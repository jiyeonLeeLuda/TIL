# 자바의 동시성 제어

## java 에서 동시성 문제를 해결하기 위한 방법으로는 어떤게 있을까요?

- volatile : volatile 키워드로 선언된 변수는 CPU의 캐시된 값이 아니라, 휘발성 메모리, 즉 메인 메모리에서 해당 변수의 값을 조회하고 씀으로써 항상 최신 상태의 값을 유지함. (이것이 source of truth?!)

- compareAndSwap: 비교한뒤 교체하는 원자(atomic) 명령으로 메모리 위치의 내용을 지정된 값과 비교하여 동일한 경우에만 해당 메모리 위치의 내용을 새로운 지정된 값으로 수정함. (조금더 공부가 필요해요!)

- synchronized : 여러스레드가 하나의 공유자원에 동시에 접근하지 못하도록, synchronized 설정된 메소드나 코드블럭은 lock권한을 가진 하나의 스레드만 독점 사용을 하는 것.

**updated 2023.03.27**

## ConcurrentHashMap의 스레드 세이프는 어떻게 가능한 것일까?

ConcurrentHashMap에 대해서 조사 하였을 때, 쓰기 작업에서 ConcurrentHashMap은 [버킷(혹은 세그먼트) 단위의 Lock](https://devlog-wjdrbs96.tistory.com/269)을 통해서 동시성을 제어하면서도 효율적으로 병렬처리가 가능하도록 했다.

오늘은 실제 ConcurrentHashMap의 코드를 열어보고 어떻게 동시성을 제어하는지 알아보려고 한다.

## put 메소드 살펴보기.

```
    public V put(K key, V value) {
        return putVal(key, value, false);
    }
```

내부에서 putVal 메소드를 호출한다.

### 동시성 제어가 필요한 순간은, 여러개의 스레드가 이미 존재하는 key에 새로운 value를 덮어씌울 때다.

이 점을 생각하며 아래 putVal메소드를 살펴보자.

```Java
   final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) throw new NullPointerException();
        int hash = spread(key.hashCode());
        int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh;
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                if (casTabAt(tab, i, null,
                             new Node<K,V>(hash, key, value, null)))
                    break;                   // no lock when adding to empty bin
            }
            else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            else {
                V oldVal = null;
                synchronized (f) {
                    if (tabAt(tab, i) == f) {
                        if (fh >= 0) {
                            binCount = 1;
                            for (Node<K,V> e = f;; ++binCount) {
                                K ek;
                                if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    if (!onlyIfAbsent)
                                        e.val = value;
                                    break;
                                }
                                Node<K,V> pred = e;
                                if ((e = e.next) == null) {
                                    pred.next = new Node<K,V>(hash, key,
                                                              value, null);
                                    break;
                                }
                            }
                        }
                        else if (f instanceof TreeBin) {
                            Node<K,V> p;
                            binCount = 2;
                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                           value)) != null) {
                                oldVal = p.val;
                                if (!onlyIfAbsent)
                                    p.val = value;
                            }
                        }
                    }
                }
                if (binCount != 0) {
                    if (binCount >= TREEIFY_THRESHOLD)
                        treeifyBin(tab, i);
                    if (oldVal != null)
                        return oldVal;
                    break;
                }
            }
        }
        addCount(1L, binCount);
        return null;
    }
```

### 1. volatile

> for (Node<K,V>[] tab = table;;)

- 해당 반복문 안에서 table이란 객체를 사용하는데, 선언부를 보면 아래와 같다.

```
transient volatile Node<K,V>[] table;
```

- ConcurrentHashMap의 대부분의 변수는 transient volatile 로 선언되있다.
  - transient사전적 의미는 일시적인, 잠시 체류하는 이란 뜻을 갖고 있고, Serialize하는 과정에 제외하고 싶은 경우 선언하는 키워드이다.
  - 즉 다른 외부 객체나, 클라이언트에게 (통신을 통해) 값을 공유하거나 전달하고 싶지 않은 데이터임을 밝히고 있다. 말 그대로 임시적으로 내부적으로 사용하는 값이란 의미로 해석된다.

#### 그렇다면 volatile란 무엇인가?

- 사전적 의미는 휘발성 물질. = 즉 램 메모리 공간을 말함
- [volatile keyword는 Java 변수를 Main Memory에 저장하겠다라는 것을 명시하는 것](https://nesoy.github.io/articles/2018-06/Java-volatile)

- 매번 변수의 값을 Read할 때마다 CPU cache에 저장된 값이 아닌 Main Memory에서 읽는 것.

- 또한 변수의 값을 Write할 때마다 Main Memory에 까지 작성하는 것.

- Multi Thread환경에서 Thread가 변수 값을 읽어올 때 각각의 CPU Cache에 저장된 값이 다르기 때문에 변수 값 불일치 문제를 해결하기 위해 쓰인다.

즉 메모리에 올라온 가장 최신의 자료를 항상 사용하겠다는 의미로, 멀티스레드 환경에서 값 불일치 문제를 최소화 하겠다는 의미이다.

### [compareAndSwap](https://en.wikipedia.org/wiki/Compare-and-swap) 원자 명령

```
else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
```

Node<K,V>객체 f 의 상수 hash가 MOVED 인경우 (? 잘은 모르겠지만 node의 이동을 일컷는 것 같음 ? )

helpTransfer <- 변형을 돕는 메소드를 호출 한다. 자료구조의 변형이 있다면 동시성 제어가 필요한 순간이다.

코드를 더 살펴보자면

```
    final Node<K,V>[] helpTransfer(Node<K,V>[] tab, Node<K,V> f) {
        Node<K,V>[] nextTab; int sc;
        if (tab != null && (f instanceof ForwardingNode) &&
            (nextTab = ((ForwardingNode<K,V>)f).nextTable) != null) {
            int rs = resizeStamp(tab.length);
            while (nextTab == nextTable && table == tab &&
                   (sc = sizeCtl) < 0) {
                if ((sc >>> RESIZE_STAMP_SHIFT) != rs || sc == rs + 1 ||
                    sc == rs + MAX_RESIZERS || transferIndex <= 0)
                    break;
                if (U.compareAndSwapInt(this, SIZECTL, sc, sc + 1)) {
                    transfer(tab, nextTab);
                    break;
                }
            }
            return nextTab;
        }
        return table;
    }
```

compareAndSwapInt으로 체크한 값을 토대로 transfer메소드를 실행하는 것을 볼 수 있다.

- compareAndSwap (CAS)는

  - 동기화를 달성하기 위해 멀티스레딩 에 사용되는 원자 명령 입니다.
  - 메모리 위치의 내용을 주어진 값과 비교하고 동일한 경우에만 해당 메모리 위치의 내용을 주어진 새로운 값으로 수정합니다.

  - 단일 원자 연산으로 수행되며, 원자성은 최신 정보를 기반으로 새 값이 계산되도록 보장합니다.
  - 그 동안 다른 스레드에서 값을 업데이트한 경우 쓰기가 실패합니다. 작업 결과는 대체를 수행했는지 여부를 간단한 부울 로 나타낼 수 있습니다.

### 3. synchronized

```
else {
                V oldVal = null;
                synchronized (f) {
```

- 그리고 익숙한 synchronized가 보인다.
- 이미 존재하는 키의 값을 덮어씌우는 경우에 해당하는 코드로 추측되며 synchronized 블럭을 통해 동시성을 제어 하고 있다.

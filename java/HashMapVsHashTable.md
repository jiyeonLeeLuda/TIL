# HashMap VS HashTable

**updated 2023.03.15**

<hr/>

### 정의

Hash table, Hash Map

> In computing, a hash table, also known as hash map, is a data structure that implements an associative array or dictionary. It is an abstract data type that maps keys to values. A hash table uses a hash function to compute an index, also called a hash code, into an array of buckets or slots, from which the desired value can be found. During lookup, the key is hashed and the resulting hash indicates where the corresponding value is stored.

> 컴퓨팅에서 해시 맵이라고도 하는 해시 테이블은 연관 배열 또는 사전을 구현하는 데이터 구조입니다. 해시 테이블은 키를 값에 매핑하는 추상 데이터 유형입니다. 해시 테이블은 해시 함수를 사용하여 원하는 값을 찾을 수 있는 버킷 또는 슬롯 배열로 인덱스(해시 코드라고도 함)를 계산합니다. 조회하는 동안 키가 해시 처리되고 결과 해시는 해당 값이 저장된 위치를 나타냅니다.

- Space, Search,Insert,Delete : O(n)

### Java class에서 [HashTable](https://docs.oracle.com/javase/8/docs/api/java/util/Hashtable.html)

> public class Hashtable<K,V>

> extends Dictionary<K,V>

> implements Map<K,V>, Cloneable, Serializable

> This class implements a hash table, which maps keys to values. Any non-null object can be used as a key or as a value.
> To successfully store and retrieve objects from a hashtable, the objects used as keys must implement the hashCode method and the equals method.

이 클래스는 키를 값에 매핑하는 해시 테이블을 구현합니다. 널이 아닌 모든 객체를 키 또는 값으로 사용할 수 있습니다.
해시 테이블에서 객체를 성공적으로 저장하고 검색하려면 키로 사용되는 객체가 hashCode 메서드와 equals 메서드를 구현해야 합니다.

> As of the Java 2 platform v1.2, this class was retrofitted to implement the Map interface, making it a member of the Java Collections Framework. Unlike the new collection implementations, Hashtable is synchronized. If a thread-safe implementation is not needed, it is recommended to use HashMap in place of Hashtable. If a thread-safe highly-concurrent implementation is desired, then it is recommended to use ConcurrentHashMap in place of Hashtable.

Java 2 플랫폼 v1.2부터 이 클래스는 Map 인터페이스를 구현하도록 개조되어 Java 컬렉션 프레임워크의 멤버가 되었습니다. 새로운 컬렉션 구현과 달리 해시 테이블은 동기화됩니다. 스레드 안전 구현이 필요하지 않은 경우 Hashtable 대신 HashMap을 사용하는 것이 좋습니다. 스레드에 안전한 고도의 동시성 구현이 필요한 경우, Hashtable 대신 ConcurrentHashMap을 사용하는 것이 좋습니다.

### Java class에서 [HashMap](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)

> public class HashMap<K,V>

> extends AbstractMap<K,V>

> implements Map<K,V>, Cloneable, Serializable

> Hash table based implementation of the Map interface. This implementation provides all of the optional map operations, and permits null values and the null key. (The HashMap class is roughly equivalent to Hashtable, except that it is unsynchronized and permits nulls.) This class makes no guarantees as to the order of the map; in particular, it does not guarantee that the order will remain constant over time.

맵 인터페이스의 해시 테이블 기반 구현입니다. 이 구현은 모든 선택적 맵 연산을 제공하며, 널 값과 널 키를 허용합니다. (HashMap 클래스는 동기화되지 않고 널을 허용한다는 점을 제외하면 해시 테이블과 거의 동일합니다). 이 클래스는 맵의 순서를 보장하지 않으며, 특히 시간이 지나도 순서가 일정하게 유지된다는 것을 보장하지 않습니다.

- HashTable은 Dictionary 클래스를 상속받고,
  - [Dictionary](https://docs.oracle.com/javase/8/docs/api/java/util/Dictionary.html) 클래스는 더이상 사용하지 않고, map 인터페이스를 구현하라고 한다.
- HashMap은 [AbstractMap](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractMap.html)을 상속 받았다.

- HashTable은 스레드 세이프하고, null을 키,값 에 허용하지 않는다.
- HashMap은 unsynchronized (스레드 세이프 하지 않고), null을 허용한다.

#### [Java HashMap은 어떻게 동작하는가?](https://d2.naver.com/helloworld/831311)

- HashMap은 보조 해시 함수(Additional Hash Function)를 사용하기 때문에 보조 해시 함수를 사용하지 않는 HashTable에 비하여 해시 충돌(hash collision)이 덜 발생할 수 있어 상대으로 성능상 이점이 있다.
- 보조 해시 함수가 아니더라도, HashTable 구현에는 거의 변화가 없는 반면, HashMap은 지속적으로 개선되고 있다
- HashMap과 HashTable을 정의한다면, '키에 대한 해시 값을 사용하여 값을 저장하고 조회하며, 키-값 쌍의 개수에 따라 동적으로 크기가 증가하는 associate array'라고 할 수 있다.

### 다른 옵션 [ConcurrentHashMap](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html)

> public class ConcurrentHashMap<K,V>

> extends AbstractMap<K,V>

> implements ConcurrentMap<K,V>, Serializable

> A hash table supporting full concurrency of retrievals and high expected concurrency for updates. This class obeys the same functional specification as Hashtable, and includes versions of methods corresponding to each method of Hashtable. However, even though all operations are thread-safe, retrieval operations do not entail locking, and there is not any support for locking the entire table in a way that prevents all access. This class is fully interoperable with Hashtable in programs that rely on its thread safety but not on its synchronization details.

검색의 전체 동시성과 업데이트에 대한 높은 기대 동시성을 지원하는 해시 테이블입니다. 이 클래스는 해시테이블과 동일한 기능 사양을 따르며, 해시테이블의 각 메서드에 대응하는 메서드 버전을 포함합니다. 그러나 모든 연산이 스레드 안전하지만 검색 연산에는 잠금이 수반되지 않으며 모든 액세스를 방지하는 방식으로 전체 테이블을 잠그는 것은 지원되지 않습니다. 이 클래스는 스레드 안전성에 의존하지만 동기화 세부 정보에는 의존하지 않는 프로그램에서 해시테이블과 완전히 상호 운용됩니다.

### 더 찾고 싶은 것

- highly-concurrent 란?
- ConcurrentModificationException 이란?
- ConcurrentHashMap 이란?
- associate array (대표적으로 Map, Dictionary, Symbol Table 등)
- 왜 Java에서는 dicionary는 더이상 사용안하고 map을 구현하라고 할까?
- 해시맵의 해시충돌과 관련하여 - String 의 hashcode(32비트 정수 자료형) 메소드의 한계

### 해시맵의 해시충돌과 관련하여 - String 의 hashcode 메소드의 한계

- 참고자료 [Java HashMap은 어떻게 동작하는가?](https://d2.naver.com/helloworld/831311)

#### 해시 분포와 해시 충돌

> 동일하지 않은 어떤 객체 X와 Y가 있을 때, 즉 X.equals(Y)가 '거짓'일 때 X.hashCode() != Y.hashCode()가 같지 않다면, 이때 사용하는 해시 함수는 완전한 해시 함수(perfect hash functions)라고 한다.

> Boolean같이 서로 구별되는 객체의 종류가 적거나, Integer, Long, Double 같은 Number 객체는 객체가 나타내려는 값 자체를 해시 값으로 사용할 수 있기 때문에 완전한 해시 함수 대상으로 삼을 수 있다.

> 하지만 String이나 POJO(plain old java object)에 대하여 완전한 해시 함수를 제작하는 것은 사실상 불가능하다.

> HashMap은 기본적으로 각 객체의 hashCode() 메서드가 반환하는 값을 사용하는 데, 결과 자료형은 int다. 32비트 정수 자료형으로는 완전한 자료 해시 함수를 만들 수 없다. 논리적으로 생성 가능한 객체의 수가 2^32보다 많을 수 있기 때문이며, 또한 모든 HashMap 객체에서 O(1)을 보장하기 위해 랜덤 접근이 가능하게 하려면 원소가 2^32인 배열을 모든 HashMap이 가지고 있어야 하기 때문이다.

> 따라서 HashMap을 비롯한 많은 해시 함수를 이용하는 associative array 구현체에서는 메모리를 절약하기 위하여 실제 해시 함수의 표현 정수 범위 N보다 작은 M개의 원소가 있는 배열만을 사용한다. 따라서 다음과 같이 객체에 대한 해시 코드의 나머지 값을 해시 버킷 인덱스 값으로 사용한다.

- 더 읽을 거리

  - [Java HashMap은 어떻게 동작하는가?](https://d2.naver.com/helloworld/831311)

## 추가 HashMap 은 어떻게 구현되어있길래 키와 값을 빠르게 매핑할 수 있을까요?

나의 답변

- 키값을 통해 값을 저장하는 버킷 인덱스 값을 알아낼 수 있기 때문에 배열에서 인덱스로 값을 꺼내 듯이 O(1) 값을 빠르게 맵핑 할 수 있습니다.
- 단, 다른 키값이 동일한 버킷 인덱스를 갖는 경우 해시 충돌이 발생했다고 하는데, 이 경우 버킷 셀 안에 목록이 저장되어 equals 함수를 통해 키를 비교해서 값을 찾아 내야 합니다. 목록을 순회 함으로 최악의 경우 O(n) 으로 값을 찾는데 시간이 더 소요 될 수 있습니다.

### hash load factor 는 무엇일까요?

- load factor를 직역하면 부하율이라 하는데, 해시맵의 버킷 입장에서는 적제율이라고도 할 수 있을것 같습니다.
- hash load factor는 hash map의 버킷에 데이터를 저장(적제)할 수 있는 적정비율을 말하며, 기본값은 0.75 (75%) 입니다. 이 값을 초과하면 hash map은 버킷수를 2배로 늘립니다.

- 참고 [HashMap 은 어떻게 구현되어있길래 키와 값을 빠르게 매핑할 수 있을까?](https://velog.io/@dailyzett/HashMap)

- 참고 [HashTable 및 HashMap 키-값은 메모리에 어떻게 저장됩니까?](https://stackoverflow.com/questions/10894558/how-hashtable-and-hashmap-key-value-are-stored-in-the-memory)

- 참고 [What makes hashmaps faster?](https://stackoverflow.com/questions/41412011/what-makes-hashmaps-faster)

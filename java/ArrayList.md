# ArrayList 심층탐구

**updated 2023.03.23**

- ArrayList 기본 크기 이상 들어왔을때

<hr/>

## ArrayList

### ArrayList 기본 크기

- ArrayList 기본 생성자 코드 열어보기.

  ```
    /**
     * Constructs an empty list with an initial capacity of ten.
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
  ```

  - 초기 용량이 10인 빈 목록을 생성합니다. 라고 주석에 써있지만 **DEFAULTCAPACITY_EMPTY_ELEMENTDATA** 라는 상수 값을 사용하고 있어 확인해 보니

  ```
      /**
     * The array buffer into which the elements of the ArrayList are stored.
     * The capacity of the ArrayList is the length of this array buffer. Any
     * empty ArrayList with elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
     * will be expanded to DEFAULT_CAPACITY when the first element is added.
     */
    transient Object[] elementData; // non-private to simplify nested class access

  ```

  - elementData는 ArrayList의 엘리먼트가 저장되는 배열 버퍼입니다. ArrayList의 용량은 이 배열 버퍼의 길이입니다.
  - elementData == DEFAULTTCAPACITY_EMPTY_ELEMENTDATA 인 빈 배열 목록은 첫 번째 요소가 추가될 때 DEFAULT_CAPACITY로 확장됩니다.

  ```
      /**
     * Default initial capacity.
     */
    private static final int DEFAULT_CAPACITY = 10;
  ```

- 즉 new ArrayList() 생성자를 통해 어레이리스트를 생성하면 빈값으로 초기화 되고, 이후 첫번째 값을 추가할때 DEFAULT_CAPACITY인 10으로 초기화 된다.

### ArrayList 기본 크기 이상 들어왔을때

- 값을 추가하는 add 메소드

  ```
      /**
     * Appends the specified element to the end of this list.
     *
     * @param e element to be appended to this list
     * @return <tt>true</tt> (as specified by {@link Collection#add})
     */
    public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
  ```

  - add메소드 내부에는 ensureCapacityInternal(size + 1); 직역하자면 **내부 용량 확인** 메소드가 있다. 더 살펴보자.

    ```
        private void ensureCapacityInternal(int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }

        ensureExplicitCapacity(minCapacity);
    }

    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;

        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
    ```

    - 이 부분이 중요한데,
      minCapacity - elementData.length > 0, 즉 현재 arrayList버퍼의 크기보다 큰용량을 필요로 하면, grow() 메소드를 호출 한다.

- grow메소드를 살펴보자

  ```
  ncreases the capacity to ensure that it can hold at least the number of elements specified by the minimum capacity argument.
  Params:
  minCapacity – the desired minimum capacity

      private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
  ```

  - 최소 용량 인수로 지정된 요소 수 이상을 보유할 수 있도록 용량을 늘립니다.
  - oldCapacity + (oldCapacity [>>](https://stackoverflow.com/questions/30004456/what-does-the-symbol-mean-in-java) 1) 코드를 풀이하지면 oldCapacitiy+ 1/2 oldCapacitiy라고 한다.

  - 즉 기본적으로 기존의 arrayList버퍼 크기의 1.5배로 용량을 증량한다.
  - 만약 최소용량(minCapacity)가 1.5배 증량한 값보다 크다면, 최소용량으로
  - 증량된 용량값이 MAX_ARRAY_SIZE값보다 크다면 hugeCapacity메소드를 통해 증량할 용량값을 구한다.

  - 그리고 elementData = Arrays.copyOf(elementData, newCapacity); 코드를 처럼 새 배열에 기존데이터를 복사해 덮어씌운다.

### arrayList add메소스 사용시 주의 할 점

> 즉, grow는 elementData가 부족할 때마다 1.5배 사이즈를 갖는 배열을 생성해 값을 복사하는 작업을 한다.

> elementData 배열의 사이즈가 작을 때에는 이런 복사작업이 좀 있어도 괜찮은데, 아이템의 수가 많아지면 그만큼 grow가 여러 차례 발생한다는 점을 고려해야 한다. 특히 아이템을 1개씩 계속해서 추가하고 있다면 grow를 통한 복사가 누적될 수 있으므로 주의해야 한다.

> 다음은 [Clojure vector](https://johngrib.github.io/wiki/clojure/study/vector/) 문서에서 인용한 것이다.
> 32801개의 아이템을 하나씩 일일이 ArrayList에 추가할 때 내부에서 새로 만드는 배열의 사이즈를 순서대로 나열해보면 다음과 같다.

> 10, 15, 22, 33, 49, 73, 109, 163, 244, 366, 549, 823, 1234, 1851, 2776, 4164, 6246, 9369, 14053, 21079, 31618, 47427

> array copy를 통한 아이템의 복사는 146,029 회.

> 그러므로 32801개의 아이템을 ArrayList에 하나 하나 추가하면 최소한 146029회의 값 복사가 발생한다. (게다가 복제 이후에 이전의 배열은 안 쓰게 되므로 gc가 모두 청소할 것이다.)

- 출처 [java.util.ArrayList 이걸 모르면 곤란하지](https://johngrib.github.io/wiki/java/arraylist/)

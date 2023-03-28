# java stream 이란?

- (파일 stream i/o 아님)

**updated 2023.03.28**

## stream 이란?

[스트림 인터페이스 문서](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)를 보자.

> A sequence of elements supporting sequential and parallel aggregate operations. The following example illustrates an aggregate operation using Stream and IntStream:

순차 및 병렬 집계 연산을 지원하는 요소의 시퀀스(연속체)입니다. 다음 예는 Stream 및 IntStream을 사용한 집계 연산을 보여줍니다:

```Java
     int sum = widgets.stream()
                      .filter(w -> w.getColor() == RED)
                      .mapToInt(w -> w.getWeight())
                      .sum();
```

(...중략...)

> To perform a computation, stream operations are composed into a stream pipeline. A stream pipeline consists of a source (which might be an array, a collection, a generator function, an I/O channel, etc), zero or more intermediate operations (which transform a stream into another stream, such as filter(Predicate)), and a terminal operation (which produces a result or side-effect, such as count() or forEach(Consumer)). Streams are lazy; computation on the source data is only performed when the terminal operation is initiated, and source elements are consumed only as needed.

계산을 수행하기 위해 스트림 연산은 스트림 파이프라인으로 구성됩니다. 스트림 파이프라인은 소스(배열, 컬렉션, 제너레이터 함수, I/O 채널 등), 0개 이상의 중간 연산(필터(Predicate)와 같이 스트림을 다른 스트림으로 변환), 터미널 연산(count() 또는 forEach(Consumer)와 같이 결과 또는 부작용을 생성하는 연산)으로 구성됩니다. 스트림은 지연형이며, 소스 데이터에 대한 계산은 터미널 연산이 시작될 때만 수행되고 소스 요소는 필요한 만큼만 소비됩니다.

> Collections and streams, while bearing some superficial similarities, have different goals. Collections are primarily concerned with the efficient management of, and access to, their elements. By contrast, streams do not provide a means to directly access or manipulate their elements, and are instead concerned with declaratively describing their source and the computational operations which will be performed in aggregate on that source. However, if the provided stream operations do not offer the desired functionality, the BaseStream.iterator() and BaseStream.spliterator() operations can be used to perform a controlled traversal.

컬렉션과 스트림은 표면적인 유사점이 있지만 서로 다른 목표를 가지고 있습니다. 컬렉션은 주로 해당 요소를 효율적으로 관리하고 액세스하는 데 중점을 둡니다. 반면 스트림은 요소에 직접 액세스하거나 조작할 수 있는 수단을 제공하지 않으며, 대신 소스와 해당 소스에서 집합적으로 수행될 계산 연산을 선언적으로 설명하는 데 중점을 둡니다. 그러나 제공된 스트림 연산이 원하는 기능을 제공하지 않는 경우 BaseStream.iterator() 및 BaseStream.spliterator() 연산을 사용하여 제어된 순회를 수행할 수 있습니다.

(...중략...)

> Stream pipelines may execute either sequentially or in parallel. This execution mode is a property of the stream. Streams are created with an initial choice of sequential or parallel execution. (For example, Collection.stream() creates a sequential stream, and Collection.parallelStream() creates a parallel one.) This choice of execution mode may be modified by the BaseStream.sequential() or BaseStream.parallel() methods, and may be queried with the BaseStream.isParallel() method.

스트림 파이프라인은 순차적으로 또는 병렬로 실행될 수 있습니다. 이 실행 모드는 스트림의 속성입니다. 스트림은 처음에 순차 또는 병렬 실행을 선택하여 생성됩니다. (예를 들어 Collection.stream()은 순차 스트림을 생성하고 Collection.parallelStream()은 병렬 스트림을 생성합니다.) 이 실행 모드 선택은 BaseStream.sequential() 또는 BaseStream.parallel() 메서드로 수정할 수 있으며, BaseStream.isParallel() 메서드로 쿼리할 수 있습니다.

## 문서 주요 포인트 요약

- 구성요소들이 순서있게 연속적으로 있는 객체.(시퀀스)
- 순차 및 병렬 집계 작업을 지원한다.

- 배열, 컬렉션, 제너레이터 함수, I/O 채널 등으로 부터 스트림 객체를 생성할 수 있다.

- 스트림 객체는 필터링,카운팅 등 중간 연산(가공)이 가능하다.

- 스트림 객체는 순차적으로 또는 병렬(parallel processing)로 실행 가능 하다.

## 사용법

### 구성

Stream객체 사용은 크게 3단계로 나누어지며, 각 파트별로 사용되는 주요 기능은 아래와 같다.

- 생성하기
  - 배열 / 컬렉션 / 빈 스트림
  - Stream.builder() / Stream.generate() / Stream.iterate()
  - 기본 타입형 / String / 파일 스트림
  - 병렬 스트림 / 스트림 연결하기
- 가공하기
  - Filtering
  - Mapping
  - Sorting
  - Iterating
- 결과만들기

  - Calculating
  - Reduction
  - Collecting
  - Matching
  - Iterating

  참고 [Java Stream 활용하기](https://moonong.tistory.com/60)

### 왜 사용할까?

Java 6 이전까지는 컬렉션 요소들을 순회하기 위해서 for문이나 forEach, while 등을 사용 했어야 했다.

Java 8부터 추가된 스트림을 사용하면 복잡한 연산이 필요한 코드도 조금 더 단순하게 작성할 수 있다.

예시

```Java

ArrayList<String> list = new ArrayList<String>(Arrays.asList("a", "b", "c"));

/* 배열의 index를 사용한 for문 */
        for(int i =0; i < list.size() ; i++){
            String value = list.get(i);

            if (value=="b"){
                System.out.println("값 : " + value);
            }
        }

/* while 이터레이터 객체 이용*/
Iterator<String> iterator = list.iterator();

while(iterator.hasNext()) {
    String value = iterator.next();

            if (value=="b"){
                System.out.println("값 : " + value);
            }
}

/* forEach 이용 */
for (String value : list) {
            if (value=="b"){
                System.out.println("값 : " + value);
            }
}

/* stream 이용 */

        list.stream()
                .filter("b"::equals)
                .forEach(x-> System.out.println("값 : "+x));

```

참고 [[Java] 자바 스트림(Stream) 사용법 및 예제](https://hbase.tistory.com/171)

## 더 읽을 거리

- [Java 스트림 Stream (1) 총정리](https://futurecreator.github.io/2018/08/26/java-8-streams/)
- [Java 스트림 Stream (2) 고급](https://futurecreator.github.io/2018/08/26/java-8-streams-advanced/)

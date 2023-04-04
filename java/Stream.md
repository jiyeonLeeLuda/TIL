# java stream 이란?

- (파일 stream i/o 아님)

**updated 2023.04.04 / 2023.03.28**

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

## 스트림의 특징 ([util.stream 설명 문서](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html))

- 저장소가 없습니다. 스트림은 요소를 저장하는 데이터 구조가 아닙니다. 대신 데이터 구조, 배열, 생성기 함수 또는 I/O 채널과 같은 소스의 요소를 계산 작업의 파이프라인을 통해 전달합니다.

- 본질적으로 기능적입니다. 스트림에 대한 작업은 결과를 생성하지만 소스를 수정하지는 않습니다. 예를 들어 Stream 컬렉션에서 얻은 것을 필터링하면 Stream소스 컬렉션에서 요소를 제거하는 대신 필터링된 요소 없이 새 항목이 생성됩니다.

- 게으름을 추구합니다. 필터링, 매핑 또는 중복 제거와 같은 많은 스트림 작업을 느리게 구현하여 최적화 기회를 노출할 수 있습니다. 예를 들어 "3개의 연속된 모음으로 첫 번째를 찾으십시오 String"는 모든 입력 문자열을 검사할 필요가 없습니다. 스트림 작업은 중간( Stream-생산) 작업과 최종(가치 또는 부작용 생산) 작업으로 나뉩니다. 중간 작업은 항상 게으르다.

- 제한이 없을 수 있습니다. 컬렉션의 크기는 유한하지만 스트림은 그럴 필요가 없습니다. limit(n)또는 와 같은 단락 작업을 통해 findFirst()무한한 스트림에 대한 계산을 유한한 시간 내에 완료할 수 있습니다.

- 소모품. 스트림의 요소는 스트림의 수명 동안 한 번만 방문됩니다. 와 마찬가지로 Iterator소스의 동일한 요소를 다시 방문하려면 새 스트림을 생성해야 합니다.

## 스트림 작업 및 파이프 라인

> 스트림 작업은 중간 작업 과 터미널 작업 으로 나뉘며 결합되어 스트림 파이프라인을 형성합니다 .

> 중간 작업은 새 스트림을 반환합니다. 그들은 항상 게으르다. 필터()와 같은 중간 연산을 실행해도 실제로 필터링을 수행하지 않고, 대신 주어진 술어와 일치하는 초기 스트림의 요소를 포함하는 새 스트림을 생성하여 순회 할 수 있습니다. 파이프라인 소스의 순회는 파이프라인의 터미널 연산이 실행될 때까지 시작되지 않습니다.

> 터미널 연산이 수행된 후에는 스트림 파이프라인이 사용된 것으로 간주되어 더 이상 사용할 수 없으며, 동일한 데이터 원본을 다시 순회해야 하는 경우 데이터 원본으로 돌아가서 새 스트림을 가져와야 합니다.

> 거의 대부분의 경우 터미널 연산은 데이터 소스 탐색과 파이프라인 처리를 완료한 후 반환합니다. 터미널 연산 중 iterator() and spliterator()만 그렇지 않은데, 이는 기존 연산이 작업에 충분하지 않은 경우 임의의 클라이언트 제어 파이프라인 순회를 가능하게 하는 'escape hatch'로 제공됩니다.

> 중간 연산은 다시 상태 비저장 연산과 상태 저장 연산으로 나뉩니다. 필터 및 맵과 같은 상태 비저장 연산은 새 요소를 처리할 때 이전에 표시된 요소의 상태를 유지하지 않으며, 각 요소는 다른 요소에 대한 연산과 독립적으로 처리될 수 있습니다. distinct 및 sorted와 같은 상태 저장 연산은 새 요소를 처리할 때 이전에 표시된 요소의 상태를 통합할 수 있습니다.

> 상태 저장소 연산은 결과를 생성하기 전에 전체 입력을 처리해야 할 수 있습니다. 예를 들어, 스트림의 모든 요소를 확인하기 전까지는 스트림을 정렬하여 어떤 결과도 생성할 수 없습니다. 따라서 병렬 계산에서 상태 저장 중간 연산을 포함하는 일부 파이프라인은 데이터에 대한 여러 번의 패스가 필요하거나 중요한 데이터를 버퍼링해야 할 수 있습니다. 상태 비저장 중간 연산만 포함된 파이프라인은 최소한의 데이터 버퍼링으로 순차적이든 병렬적이든 단일 패스로 처리할 수 있습니다.

> 또한 일부 연산은 단락 연산(short-circuiting)으로 간주됩니다. 중간 연산은 무한한 입력이 주어졌을 때 결과적으로 유한한 스트림을 생성할 수 있는 경우 단락 연산으로 간주됩니다. 터미널 연산은 무한한 입력이 주어졌을 때 유한한 시간 내에 종료될 수 있는 경우 단락 연산으로 간주됩니다. 파이프라인에 단락 연산이 있는 것은 무한 스트림 처리가 유한한 시간 내에 정상적으로 종료되기 위한 필요 조건이지만 충분 조건은 아닙니다.

## 문서 주요 포인트 요약

- Stream은 데이터를 시퀀스화 시켜 파이프라인을 통해 가공하는 api를 제공한다.
- 정확히는 Stream API로 원본 데이터의 변경없이 데이터 가공을 위한 함수를 제공한다.
- Stream 파이프라인은 크게 3가지 메소드(api)로 구성된다.

  1. Stream 생성메소드 : 데이터 소스로부터 순서가 있는 데이터 스트림을 생성한다.
  2. Stream 중간 가공 메소드 : 최종 연산전에 필요한 데이터를 준비/솎아내는 작업을 작성/지시한다. 바로 실행되지는 않는다. (최종 연산 실행시 수행) 항상 실행해야 하는 메소드는 아니다. (생략될 수 있음)
  3. Stream 최종 연산 메소드 : 최종 값을 도출하는 작업. 이 작업을 통해 데이터 소스와는 전혀 다른 값이 도출 될 수 도 있다.

- 최종연산이 실행된 스트림은 재사용 할 수 없다.

  - 스트림 파이프라인은 1회용이며, 동일한 원본을 다시 사용해야할 경우, 새로운 스트림을 생성해야 한다.

- 스트림의 느린 작업 처리는 여러 이점이 있다.

  - 예를들어 filter-map-sum 작업의 경우, 중간 상태 없이 데이터에 대한 단일 패스로 통합할 수 있어 효율이 크게 향상된다.
  - 필요하지 않은 경우를 제외함으로 모든 데이터를 검사하지 않아도 되는데, 이 동작은 입력스트림이 무한에 가까울 수록 더욱 가치를 발휘 한다.

- 중간 연산은 다시 상태 비저장 연산과 상태 저장 연산으로 나뉜다.

  > 병렬 계산에서 상태 저장 중간 연산을 포함하는 일부 파이프라인은 데이터에 대한 여러 번의 패스가 필요하거나 중요한 데이터를 버퍼링해야 할 수 있습니다.

- 배열, 컬렉션, 제너레이터 함수, I/O 채널 등으로 부터 스트림 객체를 생성할 수 있다.

- 스트림 객체는 순차적으로 또는 병렬(parallel processing)로 실행 가능 하다.

## 사용법

### 구성

Stream객체 사용은 크게 3단계로 나누어지며, 각 파트별로 사용되는 주요 기능은 아래와 같다.

- 생성하기 = 소스(배열, 컬렉션, 제너레이터 함수, I/O 채널 등)

  - 배열 / 컬렉션 / 빈 스트림
  - Stream.builder() / Stream.generate() / Stream.iterate()
  - 기본 타입형 / String / 파일 스트림
  - 병렬 스트림 / 스트림 연결하기

- 가공하기 = 중간 연산 (스트림을 다른 스트림으로 변경함)

  - Filtering
  - Mapping
  - Sorting
  - Iterating

- 결과만들기 = 터미널 연산, 최종연산

  - Calculating
  - Reduction
  - Collecting
  - Matching
  - Iterating

  참고 [Java Stream 활용하기](https://moonong.tistory.com/60)

### 왜 사용할까?

Java 6 이전까지는 컬렉션 요소들을 순회하기 위해서 for문이나 forEach, while 등을 사용 했어야 했다.

Java 8부터 추가된 스트림을 사용하면 복잡한 연산이 필요한 코드도 조금 더 단순하게 작성할 수 있다.

#### 보충 설명 (from. [Jammini's TIL](https://github.com/Jammini/TIL/blob/master/java/stream.md) )

> 자바에서는 많은 양의 데이터를 저장하기 위해 배열이나 컬렉션을 사용한다. 이렇게 저장된 데이터에 접근하기 위해서는 반복문이나 반복자(iterator)를 사용하여 매번 새로운 코드를 작성해야 한다.

> 이렇게 작성된 코드는 길이가 너무 길고 가독성도 떨어지며, 코드의 재사용이 거의 불가능하다.

> 즉, 데이터베이스의 쿼리와 같이 정형화된 처리 패턴을 가지지 못했기에 데이터마다 다른 방법으로 접근해야만 했다.

> 이러한 문제점을 극복하기 위해서 스트림 API를 도입하였다.

> 스트림 API는 데이터를 추상화해서 다루므로 다양한 방식으로 저장된 데이터를 읽고 쓰기 위한 공통된 방법을 제공한다. 따라서, 스트림 API를 이용하면 배열이나 컬렉션뿐만 아니라 파일에 저장된 데이터도 모두 같은 방법으로 다룰 수 있게 됩니다.

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

## 궁금한 점

- escape hatch란? (최종 연산 중 iterator, spliterator 사용시 다른 최종 연산과 비교하며 이해하기.)

- 스트림에서 패스란?

  > 병렬 계산에서 상태 저장 중간 연산을 포함하는 일부 파이프라인은 데이터에 대한 여러 번의 패스가 필요하거나 중요한 데이터를 버퍼링해야 할 수 있습니다.

- 무한 스트림 처리시, short-circuiting 연산이란?

## 더 읽을 거리

- [Java 스트림 Stream (1) 총정리](https://futurecreator.github.io/2018/08/26/java-8-streams/)
- [Java 스트림 Stream (2) 고급](https://futurecreator.github.io/2018/08/26/java-8-streams-advanced/)

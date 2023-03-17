# ConcurrentHashMap

**updated 2023.03.16**

<hr/>

## Intro

> If a thread-safe highly-concurrent implementation is desired, then it is recommended to use ConcurrentHashMap in place of Hashtable.

스레드에 안전한 고도의 동시성 구현이 필요한 경우, Hashtable 대신 ConcurrentHashMap을 사용하는 것이 좋습니다.

from about [Hashtable](https://docs.oracle.com/javase/8/docs/api/java/util/Hashtable.html)

## thread-safe & highly-concurrent

thread-safe란?

> thread-safety or thread-safe code in Java refers to code that can safely be utilized or shared in concurrent or multi-threading environment and they will behave as expected.

- 동시 또는 다중 스레딩 환경에서 안전하게 활용하거나 공유할 수 있으며 예상대로 동작하는 코드를 의미합니다.

Cuncurrency란?

> Concurrency is the ability to run several programs or several parts of a program in parallel. If a time consuming task can be performed asynchronously or in parallel, this improves the throughput and the interactivity of the program.

- 동시성은 여러 프로그램 또는 프로그램의 여러 부분을 병렬로 실행할 수 있는 기능입니다. 시간이 오래 걸리는 작업을 비동기 또는 병렬로 수행할 수 있으면 프로그램의 처리량과 상호 작용성이 향상됩니다.

highly-concurrent 란?

- 비동기 또는 병렬 수행을 통해 빠른 실행 속도로 프로그램이 많은 업무를 처리하는 것.

## [ConcurrentHashMap](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html) 이란?

> public class ConcurrentHashMap<K,V>

> extends AbstractMap<K,V>

> implements ConcurrentMap<K,V>, Serializable

> A hash table supporting full concurrency of retrievals and high expected concurrency for updates.

검색에 대한 최대한의 동시성 지원하고,
업데이트에 대한 높은 동시성을 기대하는 해시 테이블입니다.

> This class obeys the same functional specification as Hashtable, and includes versions of methods corresponding to each method of Hashtable.

이 클래스는 해시테이블과 동일한 기능 사양을 따르며, 해시테이블의 각 메서드에 해당하는 메서드 버전을 포함합니다

> However, even though all operations are thread-safe, retrieval operations do not entail locking, and there is not any support for locking the entire table in a way that prevents all access.

그러나 모든 작업이 스레드에 안전하더라도 검색 작업에는 잠금이 수반되지 않으며, 모든 액세스를 방지하는 방식으로 전체 테이블을 잠그는 기능은 지원되지 않습니다.

- 읽기 작업에는 잠금이 없고,
- (아마도, 수정시) 전체 테이블을 잠그는 기능이 없다는 뜻.
- 결과적으로 동시성을 최대한 고려해서 설계되었다는 의미로 해석됨.

> This class is fully interoperable with Hashtable in programs that rely on its thread safety but not on its synchronization details.

이 클래스는 스레드 안전성에 의존하지만 동기화 세부 정보에는 의존하지 않는 프로그램에서 해시 테이블과 완전히 상호 운용(서로 호환되어 사용가능)할 수 있습니다.

- 상호 운용성 : 하나의 시스템이 동일 또는 이기종의 다른 시스템과 아무런 제약이 없이 서로 호환되어 사용할 수 있는 성질을 말한다

- 동기화 세부 정보에는 의존하지 않는 프로그램 이란 무엇을 말하는가?
  - 컨커런트 해시 맵과 달리 해시 테이블은 동기화 세부 정보에 의존한다는 의미인것 같은데...
  - [해시테이블은 동시에 작업을 하려해도 객체마다 Lock을 하나씩 가지고 있기 때문에 동시에 여러 작업을 해야할 때 병목현상이 발생할 수 밖에 없습니다.(메소드에 접근하게 되면 다른 쓰레드는 Lock을 얻을 때까지 기다려야 하기 때문입니다.)](https://devlog-wjdrbs96.tistory.com/269) <= 이부분이 동기화 세부정보에 관련된 정보라 추측됨.

> Retrieval operations (including get) generally do not block, so may overlap with update operations (including put and remove). Retrievals reflect the results of the most recently completed update operations holding upon their onset.

검색 작업(get 포함)은 일반적으로 차단하지 않으므로 업데이트 작업(put 및 remove 포함)과 겹칠 수 있습니다. 검색은 시작 시점에 가장 최근에 완료된 업데이트 작업의 결과를 반영합니다.

> For aggregate operations such as putAll and clear, concurrent retrievals may reflect insertion or removal of only some entries. Similarly, Iterators, Spliterators and Enumerations return elements reflecting the state of the hash table at some point at or since the creation of the iterator/enumeration.

putAll 및 clear와 같은 집계 작업의 경우 동시 검색은 일부 항목의 삽입 또는 제거만 반영할 수 있습니다. 마찬가지로 반복자, 분할자, 열거자는 반복자/열거를 생성한 시점 또는 그 이후 해시 테이블의 상태를 반영하는 요소를 반환합니다.

> They do not throw ConcurrentModificationException. However, iterators are designed to be used by only one thread at a time. Bear in mind that the results of aggregate status methods including size, isEmpty, and containsValue are typically useful only when a map is not undergoing concurrent updates in other threads. Otherwise the results of these methods reflect transient states that may be adequate for monitoring or estimation purposes, but not for program control.

이터레이터와 분할기/열거자는 ConcurrentModificationException을 던지지 않습니다. 그러나 이터레이터는 한 번에 하나의 스레드에서만 사용하도록 설계되었습니다. size, isEmpty, containsValue를 포함한 상태 집계 메서드의 결과는 일반적으로 맵이 다른 스레드에서 동시에 업데이트되지 않는 경우에만 유용하다는 점을 유념하세요. 그렇지 않으면 이러한 메서드의 결과는 모니터링 또는 추정 목적에는 적합할 수 있지만 프로그램 제어에는 적합하지 않은 일시적인 상태를 반영합니다.

### [ConcurrentModificationException](https://docs.oracle.com/javase/8/docs/api/java/util/ConcurrentModificationException.html)이란?

- 동시-수정 예외 : 이 예외는 객체의 동시 수정을 감지한 메서드에서 해당 수정이 허용되지 않는 경우 발생할 수 있습니다.
- ConcurrentModificationException는 보통 리스트나 Map 등, Iterable 객체를 순회하면서 요소를 삭제하거나 변경을 할 때 발생합니다.

참고 [Java - ConcurrentModificationException 원인 및 해결 방법](https://codechacha.com/ko/java-concurrentmodificationexception/)

### 더 읽을거리

- [Important points about Thread-Safety in Java](https://gowthamy.medium.com/concurrent-programming-fundamentals-thread-safety-6b44c026bd2a)

- [Java concurrency (multi-threading) - Tutorial](https://www.vogella.com/tutorials/JavaConcurrency/article.html)

- [동시성이 높은 애플리케이션을 위한 설계 원칙 및 패턴](https://www.baeldung.com/concurrency-principles-patterns)

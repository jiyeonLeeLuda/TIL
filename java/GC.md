# GC

**updated 2023.02.20**

<hr>

## GC(Garbage Collection, 가비지 컬렉션)이란?

### 요약

Java에서 말하는 GC는 JVM 내부 프로그램으로, 더이상 사용하지 않는 객체에 할당된 메모리를 개발자 대신 해제 해주는 프로그램을 말한다.

- Memory Managment 관점

  - 컴퓨터 과학에서 GC는 메모리 관리의 한 형태이다. (Java에만 GC가 있는게 아니다. 다른 언어에서도 GC는 존재한다.)
  - 더 알아보기 : [위키 : Garbage collection ](<https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)>)

- Java의 메모리 관리

  - Java 내에서 메모리 관리는 JVM(Java Virtual Machine)의 일부인 가비지 수집기에 의해 처리됩니다. **_JVM 내에서 가비지 수집기는 메모리의 개체를 모니터링하는 백그라운드 프로세스입니다._**
  - 주기적으로 가비지 수집기는

    1. 메모리의 개체에 여전히 도달할 수 있는지 확인하고,
    2. 도달할 수 없는 개체를 제거하고,
    3. 메모리를 보다 효율적으로 사용하고 향후 가비지 수집을 개선하기 위해 아직 살아있는 개체를 재구성하는 가비지 수집을 실행합니다.

  - 목적
    - 가비지 컬렉터는 개발자가 메모리 관리에 소비해야 하는 시간과 노력을 상당히 줄여줍니다. 종종 개발자는 메모리 관리를 의식적으로 고려할 필요가 없습니다.
    - 가비지 수집은 또한 **_메모리 누수와 같은 문제를 제거하지는 않지만_** 크게 줄이는 데 도움이 됩니다.
  - 참고 자료 : [Introduction to Garbage Collection](https://dev.java/learn/jvm/tool/garbage-collection/intro/)

<hr>

## GC가 garbage 여부를 판단하는 기준

### 요약

진행중... (아직은 요약할 수 없음)

쓰레기 수거 프로세스 3단계!  
**_mark! sweep! and compaction!_** 표시하고, 빗질하고, 압축하라!

- #### Mark 표시하라

  - 객체 생성 시 모든 객체는 VM에 의해 1비트 표시 값으로 제공되며 처음에는 false( 0)로 설정됩니다. 이 값은 가비지 수집기가 개체에 도달할 수 있는지 여부를 표시하는 데 사용됩니다. 가비지 수집이 시작될 때 **_가비지 수집기는 개체 그래프를 순회_**하고 도달할 수 있는 모든 개체를 true( 1)로 표시합니다
  - ##### 개체 그래프 순환(traverses the object graph) 이란 ?

    - 공부 진행중...

    <img src="https://dev.java/assets/images/mark-phase-i.gif" alt="traverses the object graph" />

<hr>

## GC 종류

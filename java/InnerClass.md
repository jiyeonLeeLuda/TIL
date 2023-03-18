# Inner class 를 잘못사용하면 GC에 방해가 되는 이유는 무엇일까요?

**updated 2023.03.18**

<hr/>

## [Inner class](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html)란?

- 정의 : 클래스 내부에 선언된 클래스로, 중첩클래스라고도 한다.

- 특징

  - 이너 클래스는 자신이 선언된 아우터 클래스의 private변수에도 접근할 수 있다.
  - 이너 클래스는 자신이 선언된 아우터 클래스 외에는 접근할 수 없다.
  - 이너클래스는 아우터 클래스의 인스턴스와 연결이 되어 있기 때문에, 자체적으로 Static 멤버를 정의할 수 없다. (코드로 확인해볼 것)
  - 단, static 이너 클래스는 다른 외부 클래스 처럼 취급되어, 아우터 클래스의 멤버변수나 메소드에 직접 접근할 수 없다.

- 왜 쓸까요?

  - 캡슐화에 도움이 된다.

    - 캡슐화란 ?

      > 클래스 안에 서로 연관있는 속성과 기능들을 하나의 캡슐(capsule)로 만들어 데이터를 외부로부터 보호하는 것을 말합니다.

      즉 사람이란 클래스안에 눈 코 입등을 이너 클래스로 두어 각자의 역할에 맞는 속성과 기능을 묶어 관리할 수 있게 하고, 사람 클래스의 직접적인 맴버변수(상태)변경은 내부에서만 이루어지도록, 외부로부터 보호한다.

  - 한 곳에서만 쓰이는 클래스인 경우, 아우터 클래스에서 논리적으로 묶어 관리할 수 있다.
  - 더 읽기쉽고 유지관리에 도움이 된다.

## Inner Class 객체는 GC에서 어떻게 처리 될까?

### 나의 생각

- 이너 클래스에 선언된 변수나 메소드는 아우터 클래스의 객체가 생성되야 접근할 수 있다.
- 그러므로 이너클래스의 객체는 아우터클래스의 객체가 생성될 때, 같이 메모리에 생성된다고 생각한다.

- GC가 발생하면 Root에서부터 해당 데이터가 사용중인지 체크를 하는데, 이너클래스의 객체는 아우터클래스 객체의 참조형 멤버 변수처럼 취급될 것 이기 때문에, 그 연결을 통해 이너클래스 객체에 접근 및 멤버변수들을 순회하며 사용하는 데이터인지 확인하는 절차를 걸칠 것 같다.

- 그래서 이너클래스를 너무 많이 선언해서 사용하면, GC가 발생할때 확인해야할 이너클래스의 객체가 많기 때문에 시간이 더 소요될 것 같다.
- 또한 아우터 객체가 GC의 제거 대상이 되기 전까지 이너 클래스의 객체는 함부로 제거할 수 없음으로 힙영역을 많이 차지 할 수 있다.

- 만약 아우터 클래스의 객체가 제거 대상이 된 경우는 어떻게 처리 될까?
  - 아우터 클래스가 제거 대상이 되려면, 이너클래스의 객체가 모두 사용이 안되고 있는 것부터 확인 되어야 할 것이다.
  - 최종적으로 아우터 클래스가 제거 대상이 되면 연결된 멤버 객체들과 함께 이너 클래스도 GC 제거 대상이 된다.
  - 제거 순서는 이너클래스 객체 -> 아우터 클래스 맴버 객체 순일 것 같다.

### 이제 이 생각이 맞는지 확인해 본다.

#### 전제조건

1. 아우터 클래스의 객체가 생성될 때 이너클래스의 객체가 생성 된다. (O)

- 1번 확인 : 공식 문서

  > Objects that are instances of an inner class exist within an instance of the outer class.

  내부 클래스의 인스턴스인 객체는 외부 클래스의 인스턴스 내에 존재합니다.

2. 이너 클래스의 객체는 아우터 클래스의 멤버 변수처럼 취급된다. (O)

- 2번 확인 : 공식문서

  > An instance of InnerClass can exist only within an instance of OuterClass and has direct access to the methods and fields of its enclosing instance.

  내부 클래스의 인스턴스는 외부 클래스의 내에서만 존재할 수 있으며, 외부 클래스의 메서드와 필드에 직접 접근할 수 있다.

  - 몰랐던 점 : 아우터 클래스의 인스턴스를 생성한다고 바로 이너 클래스의 인스턴스가 생성되는 것은 아니였다. 이너 클래스의 생성도 선언해 주어야 그제서야 이너클래스의 인스턴스도 생성됨.

3. 이너 클래스의 객체는 아우터 클래스가 제거 대상이 될 때 까지 힙메모리를 차지 한다.(? 맞기도하고 틀린것 같기도 하고?)

=>아우터 클래스 인스턴스는 이너 클래스의 인스턴스가 제거 대상이 되어야 제거할 수 있음으로, 모든 이너 클래스의 인스턴스가 GC의 대상이 될 때까지 힙메모리를 차지한다.(O)

참고글 [Are non-static inner class objects garbage collected after they are no longer referenced?](https://stackoverflow.com/questions/9429121/are-non-static-inner-class-objects-garbage-collected-after-they-are-no-longer-re)

- 생성은 아우터 클래스가 먼저 선언되고, 이너 클래스의 생성을 선언해야 하지만 GC 제거는 그 반대라고 볼 수 있다.
- 이너 클래스의 객체가 살아있다면 아우터 클래스는 제거되지 않는다.
- 그러므로 아우터 클래스의 인스턴스가 제거 대상이 된다는 것은 이너클래스의 인스턴스가 제거 대상에 포함되었다는 의미이다.

### 추론이 맞았다! XD 오예!

#### Inner class 를 잘못사용하면 GC에 방해가 되는 이유는 무엇일까요? 의 결론

- 이너 클래스의 객체를 생성하게 되면, 암시적으로 아우터 클래스의 객체와 참조 관계가 되는데 (일종의 아우터 클래스의 멤버변수화) 그렇기 때문에 이너 클래스의 객체가 GC의 대상이 될 때 까지 아우터 클래스의 객체 또한 GC의 대상이 되지 않음으로 메모리 낭비가 된다.

- 요약 : inner class의 객체가 살아있으면, outer class의 객체를 GC가 수집을 못하게 함으로.

더 읽을 거리

- [Java의 내부 클래스는 static으로 선언하자](https://johngrib.github.io/wiki/java/inner-class-may-be-static/)

  - 이 글의 결론 : Java의 내부 클래스는 static으로 선언하자. 메모리를 더 먹고, 느리고, 바깥 클래스가 GC 대상에서 빠질 수 있다

- 경고: Inner class may be 'static'

  > Reports any inner classes which may safely be made static. An inner class may be static if it doesn't reference its enclosing instance.

  > A static inner class does not keep an implicit reference to its enclosing instance. This prevents a common cause of memory leaks and uses less memory per instance of the class.

  안전하게 정적으로 만들 수 있는 모든 내부 클래스를 보고합니다. 내부 클래스는 둘러싸는 인스턴스를 참조하지 않는 경우 정적일 수 있습니다.

  정적 내부 클래스는 둘러싸고 있는 인스턴스에 대한 암시적 참조를 유지하지 않습니다. 이렇게 하면 메모리 누수의 일반적인 원인을 방지하고 클래스 인스턴스당 메모리 사용량을 줄일 수 있습니다.

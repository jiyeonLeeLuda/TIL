# 추상 클래스 VS 인터페이스

**updated 2023.03.07**

참고

- [인터페이스란?](https://docs.oracle.com/javase/tutorial/java/concepts/interface.html)
- [추상 메서드 및 클래스](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
<hr/>

## 정의

### 추상클래스란 ?

- 추상적으로 선언된 클래스로써,객체를 설계할 때 클래스들의 공통적인 부분을 추출해 규격만 잡아놓은 클래스이다.
- 추상 메서드를 포함하거나 포함하지 않을 수 있다.
- 추상 클래스는 인스턴스화 할 수 없지만 하위 클래스를 만들 수 있다.

### 인터페이스란 ?

- 인터페이스는 서로 다른 두 개의 시스템, 장치 사이에서 정보나 신호를 주고받는 경우의 접점이나 경계면이다.
- 객체는 공개된 메서드를 통해 외부 세계와 상호작용을 하는데, 메세도 구현 내용은 비어있는 이러한 메소드 그룹을 인터페이스라 할 수 있다.
- 즉 특정 클래스가 어떻게 동장해야 하는지에 관한 규칙을 설정하는 일종의 프로토콜이다.

### Java 8에서 인터페이스가 변화된 점

> Interfaces can have prototypes of methods and static and final constants. But starting from Java 8, interfaces can contain static and default methods.

- 인터페이스는 메소드의 초기 형태(추상 메소드)와 static 또는 final로 선언된 상수를 갖을 수 있었습니다. 그러나 자바8부터는, 인터페이스에서 static또는 default 형태의 메소드도 표현할수 있게 되었습니다.

[Interface Enhancements In Java 8](https://www.softwaretestinghelp.com/java-8-interface-changes/)

> static and default methods 예

```
public interface DefaultStaticExampleInterface {
   default void show() {
      System.out.println("In Java 8- default method - DefaultStaticExampleInterface");
   }
   static void display() {
      System.out.println("In DefaultStaticExampleInterface I");
   }
}
```

- default 메소드 처음보는 형태. 더 알아볼 것.
- 인터페이스에 스태틱한 메소드를 추가할 수 있게 되었다는게 무엇을 의미하나?

## 추상클래스 VS 인터페이스

> 추상 클래스는 인터페이스와 유사합니다. 인스턴스화할 수 없으며 구현 여부에 관계없이 선언된 메서드의 혼합을 포함할 수 있습니다. 그러나 추상 클래스를 사용하면 정적 및 최종 필드가 아닌 필드를 선언하고 공용, 보호 및 전용 구체적인 메서드를 정의할 수 있습니다. 인터페이스를 사용하면 모든 필드가 자동으로 공개, 정적 및 최종 필드가 되며 기본 메서드로 선언하거나 정의하는 모든 메서드는 공개됩니다. 또한 추상인지 여부에 관계없이 하나의 클래스만 확장할 수 있지만 인터페이스는 얼마든지 구현할 수 있습니다.

### 인터페이스 와 추상클래스 사용 예시

<img src="https://4.bp.blogspot.com/-YX0MzkCeSmk/XKoREpikv2I/AAAAAAAANrQ/NHSp9JeUQiMxv4oteQwkknMz-_BdHmLNACLcBGAs/w483-h273/interfaces_vs_abstract_Class_Java.png">

[Difference between Abstract class and Interface in Java 8?](https://www.java67.com/2017/08/difference-between-abstract-class-and-interface-in-java8.html)

- 공통적인 제공 기능, 상호작용 방식 -> 인터페이스
- 공통적인 속성과 행위 -> 추상클래스

### 추상 클래스 또는 인터페이스 중 어느 것을 사용해야 합니까?

#### 추상 클래스 사용을 권장 할 때,

> 밀접하게 관련된 여러 클래스 간에 코드를 공유하려고 합니다.
> 추상 클래스를 확장하는 클래스에는 많은 공통 메서드 또는 필드가 있거나 public 이외의 액세스 한정자(예: protected 및 private)가 필요합니다.
> 비정적 또는 비최종 필드를 선언하려고 합니다. 이를 통해 자신이 속한 개체의 상태에 액세스하고 수정할 수 있는 메서드를 정의할 수 있습니다.

#### 인터페이스 사용을 권장 할 때,

> 다음 설명 중 하나라도 해당되는 경우 인터페이스 사용을 고려하십시오.
> 관련 없는 클래스가 인터페이스를 구현할 것이라고 예상합니다. 예를 들어, 인터페이스 Comparable및 Cloneable관련되지 않은 많은 클래스에 의해 구현됩니다.
> 특정 데이터 유형의 동작을 지정하려고 하지만 해당 동작을 구현하는 사람에 대해서는 관심이 없습니다.
> 형식의 다중 상속을 활용하려고 합니다.

### 자바에서 추상클래스와 인터페이스 사용 예시

<img src="https://www.softwaretestinghelp.com/wp-content/qa/uploads/2020/08/implementation-in-Java.png">

> 여기서 java.util.List는 인터페이스입니다. 이 인터페이스는 java.util.AbstractList에 의해 구현됩니다. 그런 다음 이 AbstractList 클래스는 LinkedList 및 ArrayList와 같은 두 가지 구체적인 클래스로 확장됩니다.
> LinkedList 및 ArrayList 클래스가 List 인터페이스를 직접 구현했다면 List 인터페이스의 모든 추상 메서드를 구현해야 합니다.
> 하지만 이 경우 AbstractList 클래스는 List 인터페이스의 메서드를 구현하고 LinkedList 및 ArrayList에 전달합니다. 그래서 여기에서 우리는 공통 동작을 구현하는 추상 클래스 유연성과 인터페이스에서 유형을 선언하는 이점을 얻습니다.

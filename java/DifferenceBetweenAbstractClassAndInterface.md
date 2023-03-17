# 추상 클래스 VS 인터페이스

**updated 2023.03.07**

참고

- [인터페이스 vs 추상클래스 (자바8 이후의 변화)](https://github.com/Jammini/TIL/blob/master/java/interface-vs-abstract.md)
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

  - 메소드 선언 시에 default를 명시하게 되면 인터페이스 내부에서도 로직이 포함된 메소드를 선언할 수 있습니다.

  - "하위 호환성"때문에 필요하다.

    - 인터페이스를 보완하는 과정에서 추가적으로 구현해야 할 혹은 필수적으로 존재해야 할 메소드가 있다면, 이미 이 인터페이스를 구현한 클래스와의 호환성이 떨어 지게 됩니다. 이러한 경우 default 메소드를 추가하게 된다면 하위 호환성은 유지되고 인터페이스의 보완을 진행 할 수 있습니다

  - [디폴트 메서드 설명1](https://velog.io/@heoseungyeon/%EB%94%94%ED%8F%B4%ED%8A%B8-%EB%A9%94%EC%84%9C%EB%93%9CDefault-Method)
  - [디폴트 메서드 설명2](https://devbksheen.tistory.com/entry/%EB%94%94%ED%8F%B4%ED%8A%B8-%EB%A9%94%EC%84%9C%EB%93%9Cdefault-method%EB%9E%80)

- 인터페이스에 스태틱한 메소드를 추가할 수 있게 되었다는게 무엇을 의미하나?

## 추상클래스 VS 인터페이스

> 추상 클래스는 인터페이스와 유사합니다. 인스턴스화할 수 없으며 구현 여부에 관계없이 선언된 메서드의 혼합을 포함할 수 있습니다. 그러나 추상 클래스를 사용하면 정적 및 최종 필드가 아닌 필드를 선언하고 공용, 보호 및 전용 구체적인 메서드를 정의할 수 있습니다. 인터페이스를 사용하면 모든 필드가 자동으로 공개, 정적 및 최종 필드가 되며 기본 메서드로 선언하거나 정의하는 모든 메서드는 공개됩니다. 또한 추상인지 여부에 관계없이 하나의 클래스만 확장할 수 있지만 인터페이스는 얼마든지 구현할 수 있습니다.

- Interface는 부모, 자식 관계인 상속 관계에 얽메이지 않고, 공통 기능이 필요 할때, Abstract Method를 정의해놓고 구현(implements)하는 Class에서 각 기능들을 Overridng하여 여러가지 형태로 구현할 수 있기에 다형성과 연관되어 있다.

  - Java에서 다중 상속이 안되어 발생하는 Abstract Class의 한계도 보완해줄 수 있다.
  - 하지만 모든 Class가 Interface를 이용한다면, 공통적으로 필요한 기능도 implements하는 모든 Class에서 Overrindg해 재정의해야 하는 번거로움이 존재한다.

  - 인터페이스는 일반변수들을 가질수 없다.

- Abstract Class는 부모와 자식 즉, 상속 관계에서 Abstract Class를 상속(extends)받으며 같은 부모 Class(여기서는 Abstract Class)를 상속받는 자식 Class들 간에 공통 기능을 각각 구현하고, 확장시키며 상속과 관련되어 있다.

- Abstract Class는 IS - A "~이다"이고, Interface는 HAS - A "~을 할 수 있는"이다.

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

## 추상 클래스와 인터페이스의 차이점

> 추상 클래스와 인터페이스는 둘 다 추상 메서드를 선언할 수 있는 추상화를 달성하는 데 사용됩니다. 추상 클래스와 인터페이스는 모두 인스턴스화할 수 없습니다. 그러나 아래에 주어진 추상 클래스와 인터페이스 사이에는 많은 차이점이 있습니다.

<table class="alt">
<tr><th>Abstract class</th><th>Interface</th></tr>
<tr><td>1) Abstract class can <strong>have abstract and non-abstract</strong> methods. 
추상 클래스는 추상 메서드와 비추상 메서드를 가질 수 있습니다</td><td>Interface can have <strong>only abstract</strong> methods. Since Java 8, it can have <strong>default and static methods</strong> also.
	인터페이스는 추상 메서드만 가질 수 있습니다 . Java 8부터 기본 및 정적 메소드 도 가질 수 있습니다 .
</td></tr>
<tr><td>2) Abstract class <strong>doesn't support multiple inheritance</strong>.추상 클래스는 다중 상속을 지원하지 않습니다 .</td><td>Interface <strong>supports multiple inheritance</strong>.인터페이스는 다중 상속을 지원합니다 .</td></tr>
<tr><td>3) Abstract class <strong>can have final, non-final, static and non-static variables</strong>.
 추상 클래스는 최종, 비최종, 정적 및 비정적 변수를 가질 수 있습니다 .
</td><td>Interface has <strong>only static and final variables</strong>.
 인터페이스에는 정적 및 최종 변수만 있습니다 .
</td></tr>
<tr><td>4) Abstract class <strong>can provide the implementation of interface</strong>.추상 클래스는 인터페이스의 구현을 제공할 수 있습니다</td><td>Interface <strong>can't provide the implementation of abstract class</strong>.인터페이스는 추상 클래스의 구현을 제공할 수 없습니다 </td></tr>
<tr><td>5) The <strong>abstract keyword</strong> is used to declare abstract class.abstract 키워드는 추상 클래스를 선언하는 데 사용됩니다.</td><td>The <strong>interface keyword</strong> is used to declare interface.interface 키워드는 인터페이스를 선언하는 데 사용됩니다.</td></tr>
<tr><td>6) An <strong>abstract class</strong> can extend another Java class and implement multiple Java interfaces.추상 클래스는 다른 Java 클래스를 확장하고 여러 Java 인터페이스를 구현할 수 있습니다.</td><td>An <strong>interface</strong> can extend another Java interface only.인터페이스 는 다른 Java 인터페이스만 확장할 수 있습니다.</td></tr>
<tr><td>7) An <strong>abstract class</strong> can be extended using keyword "extends".키워드 "extends"를 사용하여 추상 클래스를 확장할 수 있습니다.</td><td> An <strong>interface</strong> can be implemented using keyword "implements".인터페이스 는 키워드 "implements"을 사용하여 구현할 수 있습니다.</td></tr>
<tr><td>8) A Java <strong>abstract class</strong> can have class members like private, protected, etc.Java 추상 클래스는 private, protected 등과 같은 클래스 멤버를 가질 수 있습니다.</td><td>Members of a Java interface are public by default. Java 인터페이스의 구성원은 기본적으로 공개됩니다.</td></tr>
<tr><td>9)<strong>Example:</strong><br> public abstract class Shape{<br>public abstract void draw();<br>}</td><td><strong>Example:</strong><br> public interface Drawable{<br>void draw();<br>}</td></tr>
</table>

> 간단히 말해서 추상 클래스는 부분 추상화(0 ~ 100%)를 달성하는 반면 인터페이스는 완전한 추상화(100%)를 달성합니다.

출처 [추상 클래스와 인터페이스의 차이점](https://www.javatpoint.com/difference-between-abstract-class-and-interface)

## 추상클래스와 인터페이스 차이점 요약

- 인터페이스는 한번의 확장을 하는 추상클래스와 달리 여러개의 인터페이스를 구현 할 수 있다.
- 인터페이스에 선언된 메소드, 변수들은 모두 기본적으로 모두 접근 가능하도록 공개 된다.
- 인터페이스는 static이자 final 변수만 선언할 수 있어 결과적으로 관리(변경)할 수 있는 상태 값이 없다.

- 인터페이스는 기능의 목록으로 has-a 의 관점으로 해석되고, 느슨한 결합을 하는 반면, 추상클래스는 특성과 행동을 공유하는 집합채로써 is-a 관점으로 해석되며 강한 결합을 이루게 된다.

- 자바 8부터는 static, default 메소드를 지원하여, 인터페이스에서도 구현된 메소드를 선언할 수 있다.
- default 메소드는 하위 호환을 위해 추가된 기능으로, 이미 인터페이스를 구현한 클래스가 존재하며 공통기능을 수행하는 메소드가 추가 되어야 할 때 유용하다.

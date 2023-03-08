# Equals VS Hashcode

**updated 2023.03.08**

참고

- [equals​](<https://docs.oracle.com/javase/10/docs/api/java/lang/Object.html#equals(java.lang.Object)>)
- [hashCode](<https://docs.oracle.com/javase/10/docs/api/java/lang/Object.html#hashCode()>)
- [equals와 hashCode는 왜 같이 재정의해야 할까?](https://tecoble.techcourse.co.kr/post/2020-07-29-equals-and-hashCode/)
<hr/>

## 두 메소드의 기능

### equals

> Indicates whether some other object is "equal to" this one.
> (...)
> for any non-null reference values x and y, this method returns true if and only if x and y refer to the same object

다른 개체가 이 개체와 "동일"한지 여부를 나타냅니다.

null이 아닌 참조 값 x 및 y에 대해 이 메서드는 x와 y가 동일한 개체를 참조하는 경우에만 true를 반환합니다

> Note that it is generally necessary to override the hashCode method whenever this method is overridden, so as to maintain the general contract for the hashCode method, which states that equal objects must have equal hash codes.

일반적으로 이 메서드가 재정의될 때마다 hashCode 메서드를 재정의하여 동일한 개체에 동일한 해시 코드가 있어야 한다고 명시한 **hashCode 메서드에 대한 일반 계약을 유지 관리해야 합니다.**

### hashCode

> Returns a hash code value for the object.

개체의 해시 코드 값을 반환합니다.

#### The general contract of hashCode is:

> Whenever it is invoked on the same object more than once during an execution of a Java application, the hashCode method must consistently return the same integer, provided no information used in equals comparisons on the object is modified. This integer need not remain consistent from one execution of an application to another execution of the same application.

Java 응용 프로그램을 실행하는 동안 동일한 개체에서 두 번 이상 호출될 때마다 hashCode 메서드는 개체에 대한 같음 비교에 사용되는 정보가 수정되지 않는 한 **동일한 정수를 일관되게 반환해야 합니다.** 이 정수는 응용 프로그램의 한 실행에서 동일한 응용 프로그램의 다른 실행까지 일관성을 유지할 필요가 없습니다.

> If two objects are equal according to the equals(Object) method, then calling the hashCode method on each of the two objects must produce the same integer result.

두 개체가 equals(Object) 메서드에 따라 동일한 경우 두 개체 각각에 대해 hashCode 메서드를 호출하면 동일한 정수 결과가 생성되어야 합니다.

## 왜 equals와 hashCode는 같이 세트로 재정의 해야할까?

- 위 질문을 => equals와 hashCode를 같이 세트로 재정의 하지 않으면 어떻게 될까? 로 바꿔서 생각해 볼 수 있다.

- equals만 재정의하고 hashCode를 재정의 하지 않은 경우 : [예시가 잘 정리된 블로그 링크](https://tecoble.techcourse.co.kr/post/2020-07-29-equals-and-hashCode/)

> hashCode를 equals와 함께 재정의하지 않으면 코드가 예상과 다르게 작동하는 위와 같은 문제를 일으킨다. 정확히 말하면 hash 값을 사용하는 Collection(HashSet, HashMap, HashTable)을 사용할 때 문제가 발생한다.

왜냐하면

> hash 값을 사용하는 Collection(HashMap, HashSet, HashTable)은 객체가 논리적으로 같은지 비교할 때, hashCode 메서드의 리턴 값이 우선 일치하고 equals 메서드의 리턴 값이 true여야 논리적으로 같은 객체라고 판단한다.

### 요약정리

- equals는 기본적으로 비교하는 두 객체가 동일한 참조값을 향하고 있으면 true를 반환한다.
- equals메소드에서 동일하다고 판단된 객체로부터 hashCode를 호출하면 동일한 정수 결과가 나와야 한다.
- 이것이 일반적인 hashCode의 계약사항이다. (지침이다)
- 그렇기 때문에 일반적인 상황에서 equals를 재정의 할 경우, hashCode도 같은 맥락으로 재정의 하는 것이 안전한 선택이다.

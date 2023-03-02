# 부동소수점(floating point)

**updated 2023.03.02**

참고

- [부동소수점 - 위키백과](https://ko.wikipedia.org/wiki/%EB%B6%80%EB%8F%99%EC%86%8C%EC%88%98%EC%A0%90)
- [고정 소수점과 부동 소수점의 차이점](https://pediaa.com/difference-between-fixed-point-and-floating-point/)
- [Java Number Class](https://docs.oracle.com/javase/8/docs/api/java/lang/Number.html)
- [Java, BigDecimal 사용법 정리](https://jsonobject.tistory.com/466)
<hr/>

## 부동소수점(floating point)이란 무엇인가요?

### By 위키피디아

부동소수점(浮動小數點, floating point) 또는 떠돌이 소수점 방식은 실수를 컴퓨터상에서 근사하여 표현할 때 소수점의 위치를 고정하지 않고 그 위치를 나타내는 수를 따로 적는 것으로, 유효숫자를 나타내는 가수(假數)와 소수점의 위치를 풀이하는 지수(指數)로 나누어 표현한다.

컴퓨터에서는 고정 소수점 방식보다 넓은 범위의 수를 나타낼 수 있어 과학기술 계산에 많이 이용되지만, 근삿값으로 표현되며 고정 소수점 방식보다 연산 속도가 느리기 때문에 별도의 전용 연산 장치를 두는 경우가 많다. 고정 소수점과 달리 정수 부분과 소수 부분의 자릿수가 일정하지 않으나, 유효 숫자의 자릿수는 정해져 있다

이해한 특징으로는

- 가수 와 지수로 구성되어있고
- 근삿값으로 표현 된다.
- 고정 소수점과 달리 정수/소수 자릿수가 일정하지 않으나 유효 숫자의 자릿수는 정해져 있다.

### IEEE 754의 부동 소수점 표현

 <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/88/General_floating_point_ko.svg/500px-General_floating_point_ko.svg.png">

- IEEE 754의 부동 소수점 표현은 크게 세 부분으로 구성되는데, 최상위 비트는 부호를 표시하는 데 사용되며, 지수 부분(exponent)과 가수 부분(fraction/mantissa)이 있다.
- 부동 소수점 표현 방식은 수를 (가수)×(밑수)(지수)와 같이 유효숫자를 사용한 곱셈 형태로 표현한다.

## 근사값인 부동소수점, 왜 필요할까? 왜 생겼을까?

부동소수점을 이해하기 위해 반대격인 고정 소수점 (fixed point)에 대해서도 알아보자.

### 고정 소수점이란?

고정소수점은 소수점을 사용하여 고정된 자리수의 소수를 나타내는 것이다. 한정된 메모리에서 부동소수점 방식보다 좁은 범위의 수만 나타낼 수 있다.

### => 고정 소수점 보다 더 정밀한 소수를 나타내기 위해서 나온 것.

## 고정 소수점과 부동 소수점의 차이점.

고정 소수점과 부동 소수점은 숫자를 나타내는 두가지 방법이다.

고정 소수점 : 고정된 자릿수가 있는 숫자에 대한 데이터를 표현한 것.
부동 소수점 : 범위와 정밀도 간의 균형을 지원하기 위해 근사값으로 실수를 공식을 통해 표현한 것.

## java 에서 100자리 이상의 수를 계산하기 위한 클래스로는 무엇이 있을까요?

Class Number의 자식 클래스들 :

> AtomicInteger, AtomicLong, BigDecimal,
> BigInteger, Byte, Double, DoubleAccumulator,
> DoubleAdder, Float, Integer, Long,
> LongAccumulator, LongAdder, Short

답은 BigDecimal, BigInteger 아닐까요?

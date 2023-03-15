# StringBuilder VS StringBuffer

**updated 2023.03.15**

<hr/>
String은 내부의 문자열을 수정할 수 없어 변경이 생기면 힙 메모리에 새로운 string 객체가 만들고 해당 객체를 참조하게 된다.

즉, 수정을 할 수록 새로운 객체를 만들게 되어 메모리 낭비가 심하다.

StringBuilder, StringBuffer는 String의 단점을 보완하고자 나온 클래스이다.

(그래서 자바에서는 문자열에 + 연산을 사용하면 컴파일전 내부적으로 StringBuilder클래스를 만든 후 다시 문자열로 돌려준다.)

### StringBuilder, StringBuffer 공통

- 버퍼 (데이터를 임시로 저장하는 메모리)에 문자열을 저장한다.
- 버퍼 내부에서 추가,수정,삭제 작업을 할 수 있다.

### StringBuilder, StringBuffer 차이

- 멀티스레드 환경에서 :
  - StringBuffer는 스레드 세이프 하다.
  - StringBuilder는 스레드 세이프 하지 않다.
- 속도면에서 :
  - StringBuilder가 StringBuffer보다 더 빠르다.

참고 [String / StringBuffer / StringBuilder 차이점 & 성능 비교](https://inpa.tistory.com/entry/JAVA-%E2%98%95-String-StringBuffer-StringBuilder-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EC%84%B1%EB%8A%A5-%EB%B9%84%EA%B5%90)

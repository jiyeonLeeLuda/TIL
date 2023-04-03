# Oauth 2.0 과 JWT

**updated 2023.04.03**

## 정의

### Oauth 2.0 이란?

OAuth("Open Authorization")는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있게 공통적인 수단으로 사용되는 접근 위임 개방형 표준이다.

## 구성요소 (역할)

### Resource Owner(자원 소유자)

- 보호된 리소스에 대한 액세스를 승인(로그인)할 수 있는 엔티티
- 실제 앱을 사용하는 사용자라고 할 수 있다.

### Client

- Resource Owner의 보호된 리소스를 요청하는 애플리케이션.

### Authorization Server

- Resource Owner를 성공적으로 인증하고, 권한을 얻으면 Client에게 Access Toeken을 발급하는 서버

### Resource Server

- 허용할 수 있는 보호된 리소스를 호스팅하는 서버
- Access Token을 통해 보호된 리소스 요청을 수락하고 그 리소스를 응답에 담아 보내주는 서버

## 프로토콜 흐름

```text
     +--------+                               +---------------+
     |        |--(A)- Authorization Request ->|   Resource    |
     |        |                               |     Owner     |
     |        |<-(B)-- Authorization Grant ---|    (사용자)     |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- Authorization Grant -->| Authorization |
     | Client |                               |     Server    |
     |  (앱)   |<-(D)----- Access Token -------|    (인증서버)   |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)----- Access Token ------>|    Resource   |
     |        |                               |     Server    |
     |        |<-(F)--- Protected Resource ---|  (리소스 서버)   |
     +--------+                               +---------------+

                     Figure 1: Abstract Protocol Flow
```

- (A) Client는 Resource Owner에게 권한(Authorization)을 요청한다.

  - ex. 구글로 로그인 하기 버튼 클릭
  - 이 승인은 직접 혹은 Authorization Server를 통해 간접적으로 이루어질 수 있다.

- (B) Resource Owner가 승인 시 Client는 Authorization Grant(권한 증서)를 받는다. Authorization Grant 방식은 다양하며 클라이언트의 요청이나 인증 서버가 지원하는 유형에 따라 달라질 수 있다.

- (C) Client는 인증서버에 Authorization Server에 인증과 권한 증서를 제시함으로써 Access Token을 요청한다.

- (D) Authorization Server는 Client를 검증하고 권한 증서가 유요 하면 Access Token을 발급한다.

- (E) Client는 Access Token을 제시함으로써 Resource Server에 보호된 리소스를 요청한다

- (F) 리소스 서버는 그 Access Token을 검증하고 검증되었다면 서버에게 보호된 리소스를 전송한다.

### Authorization Grant란?

권한부여 방식의 종류는 4가지가 있음.

1. Authorization Code Grant
   Client가 다른 사용자 대신 특정 리소스에 접근을 요청할 때 사용한다. 리소스 접근을 위한 사용자명과 비밀번호, 권한 서버에 요청해서 받은 권한 코드를 함께 활용하여 리소스에 대한 Access Token을 받으면 이를 인증에 사용하는 방식

2. Implicit Grant
   Authorization Code Grant와 다르게 Authorization Code 교환 단계 없이 Access Token을 즉시 반환받아 이를 인증에 이용하는 방식이다.
   많이 사용되는 방식으로 모바일 애플리케이션이나 자바스크립트 기반의 웹 애플리케이션에서 사용

3. Resource Owner Password Credentials Grant
   Client가 암호를 사용하여 Access Token에 대한 사용자의 자격증명을 교환하는 방식
   Clinet에 아이디/암호를 저장해 놓고 직접 Access Token을 받는 방식이다.

4. Client Credentials Grant
   Client가 Context 외부에서 Access Token을 얻어 특정 리소스에 접근을 요청할 때 사용하는 방식

## 전체 플로우 참고 이미지 (로그인부터 ~ 리소스얻기 까지)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FHGu3i%2Fbtq2kqQrO28%2FytMUrO1opx8UJyCKOfROBK%2Fimg.png" />
출처: PAYCO 개발자 센터

<hr/>

참고

- [OAuth 2.0 - RFC 6749](https://www.rfc-editor.org/rfc/rfc6749#section-1.2)
- [OAuth 2.0, JWT](https://sun-22.tistory.com/44)
- [OAuth 2.0 개념 정리](https://hwannny.tistory.com/92)

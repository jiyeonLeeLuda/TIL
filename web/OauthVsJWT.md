# Oauth 2.0 과 JWT

**updated 2023.04.05 /2023.04.03**

## 정의

### Oauth 2.0 이란?

OAuth("Open Authorization")는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있게 공통적인 수단으로 사용되는 접근 위임 개방형 표준이다.

OAuth를 사용하면 Facebook 및 Google과 같은 타사 서비스에서 사용자의 계정 자격 증명을 타사에 노출하지 않고 최종 사용자 계정 정보를 사용할 수 있다.
최종 사용자를 대신하여 중개자 역할을 하여 특정 계정 정보를 공유하도록 승인된 타사 서비스에 액세스 토큰을 제공한다. 토큰을 얻는 프로세스를 권한 부여 흐름이라고 한다.

### JWT 란?

JSON 웹 토큰(JSON Web Token, JWT)은 선택적 서명 및 선택적 암호화를 사용하여 데이터를 만들기 위한 인터넷 표준이다. 일반적으로 클라이언트와 서버 두 엔터티 간에 정보를 공유하기 위한 개방형 산업 표준으로,
JWT에는 공유해야 하는 정보가 있는 JSON 개체가 포함되어 있다. 또한 각 JWT는 암호화 서명되어 클라이언트나 악의적인 당사자가 JSON 콘텐츠(JWT 클레임이라고도 함)를 수정할 수 없다.

## OAuth VS JWT

- OAuth 에서 발급하는 AccessToken은 항상 JWT는 아니다.

- 두 프로토콜은 목적과 사용 사례가 다르며 구분되어진다.

- JWT

  - JWT는 서버-클라이언트 간에 정보를 JSON 개체로 전송하는 작고 독립적인 방법으로, 정보를 안전하게 전송하는 데 자주 사용된다.
  - 일반적으로 사용자를 인증하고 사용자 데이터 및 파일과 같은 리소스에 대한 권한 있는 액세스를 제공하는 데 사용된다.
  - 클라이언트가 서버에서 세션 상태를 유지하지 않고도 사용자를 인증하고 리소스에 대한 액세스 권한을 부여할 수 있으므로 상태 비저장 애플리케이션에 적합하다.

- OAuth
  - 서버에서 세션 상태를 유지하고 고유한 토큰을 사용하여 사용자 리소스에 대한 액세스 권한을 부여한다.
  - 이를 통해 사용자는 사용자 이름과 암호를 제공하지 않고도 다른 사이트의 리소스에 대한 타사 응용 프로그램 액세스 권한을 부여할 수 있다.
  - (ex. Google 계정을 사용하여 음악 스트리밍 서비스에 로그인하는 것과 같이 사용자가 다른 사이트에서 자신의 계정을 사용하여 타사 애플리케이션에 로그인할 수 있도록 허용하는 데 사용)

### 요약

- 사용 사례 – JWT는 API에 더 적합합니다. OAuth는 웹, API, 브라우저 애플리케이션 및 리소스에 유용합니다.

- 토큰 – JWT는 토큰 형식을 정의합니다. OAuth는 인증 프로토콜을 정의합니다.

- 유용성 – JWT는 초기 단계부터 배우고 사용하기가 더 쉽습니다. OAuth는 더 복잡합니다.

- 저장소 – JWT는 클라이언트 측 저장소만 사용할 수 있습니다. OAuth는 서버측 및 클라이언트측 스토리지를 모두 사용할 수 있습니다.

- 범위 – JWT는 더 적은 사용 사례를 처리하고 범위가 더 작습니다. OAuth는 보다 유연하고 다양한 사용 사례에 쉽게 사용할 수 있습니다.

## OAuth2와 함께 JWT 사용

> JWT와 OAuth2는 서로 다른 용도로 사용되지만 호환되며 함께 사용할 수 있습니다. OAuth2 프로토콜은 토큰 형식을 지정하지 않기 때문에 JWT를 OAuth2 사용에 통합할 수 있습니다.

- 예시

  OAuth2 권한 부여 서버에서 반환된 access_token은 페이로드에 추가 정보를 전달하는 JWT일 수 있다. 이렇게 하면 리소스 서버와 인증 서버 간에 필요한 왕복이 줄어들어 성능이 향상될 수 있다.

- 오해 와 진실
  > 일반적인 오해는 JWT를 OAuth2와 함께 사용하면 애플리케이션 보안이 강화된다는 것이지만 반드시 그런 것은 아닙니다. 다른 표준과 마찬가지로 JWT는 뚫을 수 없는 메커니즘이 아닙니다. OAuth2 보안은 권한 부여 프로세스에 관련된 행위자와 다양한 사용 사례에서 이 프로세스에 대해 수행할 특정 단계를 정의하여 유지됩니다. OAuth2의 보안 문제는 토큰 유형이 아닌 사용 사례에 따라 애플리케이션에 적합한 OAuth2 권한 부여 흐름을 선택하여 가장 잘 해결됩니다.

## 추가 알아보기

### accessToken은 항상 JWT 형식일까? No.

- 카카오 로그인의 경우
  > 카카오 로그인은 OAuth 2.0 표준 규격에 따라 액세스 토큰(Access token), 리프레시 토큰(Refresh token) 두 종류의 토큰을 발급합니다. OpenID Connect 사용 시, 사용자 인증 정보를 담은 ID 토큰을 함께 발급합니다
  - [ID 토큰](https://developers.kakao.com/docs/latest/ko/kakaologin/common#oidc-id-token)의 경우 JWT 형식의 토큰이라 밝히고 있음.
  - [엑세스 토큰과 리프레시 토큰을 발급받는 리스폰스의 정보](https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api#request-token-response)를 확인 했을 때, 만료 시간(초)을 따로 제공한다.
    - 이를 통해 미루어 짐작하자면, 카카오 로그인 엑세스 토큰의 값은 JWT가 아닐 확률이 높다.
  - 카카오 토큰 예시
    ```JSON
    {
     "access_token":"oV6_Hn5-m6HLdnC5dLmUHOaCPb5Arp8MAfjWoAorDKcAAAF3YfGgCw",
    "token_type":"bearer",
     "refresh_token":"KSyFqrA7J2v8jGX4cn_urzZs7bT8BB_rN_7fYAorDKcAAAF3YfGgCg",
    "expires_in":21599,
    "scope":"account_email profile",
    "refresh_token_expires_in":5183999
    }
    ```
  - https://jwt.io/ 에서 Decoded를 해보았을 때, Invalid Signature 표시가 되었다.
  - 즉 카카오 로그인의 OAuth 2.0 accessToken은 JWT가 아니였다.

### accessToken 의 적정한 유효시간은 ?

- [카카오 로그인 엑세스 토큰의 만료 시간](https://developers.kakao.com/docs/latest/ko/kakaologin/common#token)은 어떻게 되는가 ?
  - Android, iOS : 12시간
  - JavaScript: 2 시간
  - REST API : 6시간
- 카카오 로그인 리프레시 토큰의 경우

  - 만료시간 2달이며,
  - 만료 시간 1달 남은 시점부터 갱신 가능.

- [oauth.com 에서 설명하는 엑세스 토큰 수명](https://www.oauth.com/oauth2-servers/access-tokens/access-token-lifetime/)

  > 서비스에서 액세스 토큰을 발급하면 토큰을 얼마나 오래 사용할 것인지 결정해야 합니다. 불행히도 모든 서비스에 대한 포괄적인 솔루션은 없습니다. 다양한 옵션과 함께 제공되는 다양한 장단점이 있으므로 애플리케이션의 요구에 가장 적합한 옵션(또는 옵션 조합)을 선택해야 합니다.

  - 일반적인 방법은 최대 보안 및 유연성을 위해 액세스 토큰과 리프레시 토큰의 조합을 사용하는 것입니다.
    - 상대적으로 짧은 엑세스토큰 유효시간 & 긴 리프레시 토큰 유효시간 설정을 통해 엑세스 토큰 만료시 리프레시 토큰으로 새로운 엑세스 토큰을 얻는다.
      - 이 접근 방식의 주요 이점은 서비스가 데이터베이스 조회 없이 확인할 수 있는 자체 인코딩된 액세스 토큰을 사용할 수 있다는 것입니다.
      - 그러나 동시에 이는 엑세스 토큰을 직접 만료할 수 없음을 의미함으로 짦은 만료시간을 설정하여, 지속적으로 토큰을 새로고쳐야 하도록 유도해야 합니다. 이를 통해 [애플리케이션의 엑세스를 취소할 수 있는 기회](https://www.oauth.com/oauth2-servers/listing-authorizations/revoking-access/)를 제공 합니다.

### refreshToken은 최대한 안전한 곳에 저장하려면?

답변 참고 : [Where to store the refresh token on the Client?](https://stackoverflow.com/questions/57650692/where-to-store-the-refresh-token-on-the-client)

- 방안 1. refreshToken을 사용하지 않는다.

  - 엑세스 토큰을 메모리에 보관하고 엑세스 토큰이 만료되면 자동 로그인을 수행.

- 방안 2. Http 전용 쿠키에 저장한다. (Http-Only)

  - Http 전용 쿠키는 API에서 반환되며 스크립트에서 읽을 수 없는 쿠키입니다! 따라서 앱에 처음 인증할 때 토큰을 반환하는 대신 쿠키를 반환합니다.
  - 쿠키에는 Url 필드가 있으며 대부분의 클라이언트 라이브러리(예: Axios, Apollo)는 요청을 보내는 URL과 일치하는 모든 쿠키를 전달합니다.
  - 즉, 특정 경로에만 서버로 전송하며, 스크립트에서 읽을 수 없는 Http 전용 쿠키를 사용할 것!

- 더 읽을거리
  - [OAuth 2.0 암시적 흐름이 중단되었나요?](https://developer.okta.com/blog/2019/05/01/is-the-oauth-implicit-flow-dead)
  - [SPA를 위한 간단하고 안전한 API 인증](https://medium.com/@sadnub/simple-and-secure-api-authentication-for-spas-e46bcea592ad)

참고

- [Access Token Lifetime](https://www.oauth.com/oauth2-servers/access-tokens/access-token-lifetime/)
- [Json 웹 토큰](https://ko.wikipedia.org/wiki/JSON_%EC%9B%B9_%ED%86%A0%ED%81%B0)
- [OAuth](https://ko.wikipedia.org/wiki/OAuth)
- [OAuth와 JWT: 차이점은 무엇입니까? 함께 사용할 수 있습니까?](https://frontegg.com/blog/oauth-vs-jwt)

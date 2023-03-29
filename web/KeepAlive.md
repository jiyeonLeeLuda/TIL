# Keep Alive 란?

**updated 2023.03.29**

## 정의

- Keep Alive는 서버 응답 헤더에서 설정하는 값중에 하나로,
- 서버와 클라이언트 사이의 연결이 유효한지 확인하거나 연결이 끊어지는 것을 방지하기 위해 한 장치(서버)에서 다른 장치(클라이언트)로 보내는 메시지로,
- 송신자(서버)가 연결에 대한 타임아웃과 요청 최대 개수를 어떻게 정했는지에 대해 알려준다.

## 왜 등장한 기술인가?

### HTTP1.x의 커넥션 관리

- 기본적으로 HTTP 통신은 무상태(stateless), 비연결성(connectionless)을 전제함으로, 클라이언트는 필요시마다 새로운 연결을 맺고 요청을 보내야 한다.

- 이러한 단기 커넥션은 두가지 단점이 있는데

  - 새로운 연결을 맺는데 드는 시간이 상당하다는 것과
  - TCP기반 커넥션의 성능은 오직 커넥션이 예열된 상태일 때만 나아진다는 것이다.

- HTTP 1.x에서는 이러한 한계를 극복하고자 [2가지 커넥션 관리 방법을 추가 했다.](https://developer.mozilla.org/ko/docs/Web/HTTP/Connection_management_in_HTTP_1.x)

  - 영속적인 커넥션 모델 (persistent connection)
  - HTTP 파이프라이닝 (http pipelining)
  - 비교 그림
    <img src="https://developer.mozilla.org/en-US/docs/Web/HTTP/Connection_management_in_HTTP_1.x/http1_x_connections.png" />

- Keep Alive는 이 두가지 방법을 구현하는데 사용되는 설정값 이다.

### 어떤 이점을 주는가?

- 영속적인 커넥션

  - 연결을 열어놓고 여러 요청에 재사용함으로써, 새로운 TCP 핸드셰이크를 하는 비용을 아끼고, TCP의 성능 향상 기능을 활용할 수 있습니다.

- HTTP 파이프라이닝

  - 영속적인 커넥션을 통해서, 응답을 기다리지 않고 요청을 연속적으로 보내는 기능입니다. 이것은 커넥션의 지연를 회피하고자 하는 방법입니다. 이론적으로는, 두 개의 HTTP 요청을 하나의 TCP 메시지 안에 채워서(be packed) 성능을 더 향상시킬 수 있습니다.

- 정리하자면
  - 네트워크 혼잡도를 감소 시키며 (TCP 커넥션 리퀘스트 수 감소)
  - 네트워크 비용을 아끼며 (한개의 커넥션으로 한명의 클라이언트의 여러 요청을 처리함으로, 여러 커넥션으로 한명의 요청들을 처리하는 것 보다 효율적)
  - 지연시간이 감소 된다. (3-way handshake를 맺을때 필요한 round-trip에 걸리는 시간의 감소)

## 예시 및 설정 값

```
HTTP/1.1 200 OK
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
Date: Thu, 11 Aug 2016 15:23:13 GMT
Keep-Alive: timeout=5, max=1000
Last-Modified: Mon, 25 Jul 2016 04:32:39 GMT
Server: Apache

(body)
```

> Connection: Keep-Alive

> Keep-Alive: timeout=5, max=1000

- timeout: 유휴 연결이 계속 열려 있어야 하는 최소한의 시간(초 단위)을 가르킵니다. keep-alive TCP 메시지가 전송 계층에 설정되지 않는다면 TCP 타임아웃 이상의 타임아웃은 무시된다는 것을 알아두시기 바랍니다.

- max: 연결이 닫히기 이전에 전송될 수 있는 최대 요청 수를 가리킵니다. 만약 0이 아니라면, 해당 값은 다음 응답 내에서 다른 요청이 전송될 것이므로 비-파이프라인 연결의 경우 무시됩니다. HTTP 파이프라인은 파이프라이닝을 제한하는 용도로 해당 값을 사용할 수 있습니다.

  - [HTTP 파이프라이닝](https://developer.mozilla.org/ko/docs/Web/HTTP/Connection_management_in_HTTP_1.x#http_%ED%8C%8C%EC%9D%B4%ED%94%84%EB%9D%BC%EC%9D%B4%EB%8B%9D) 이란?

    > 파이프라이닝이란 같은 영속적인 커넥션을 통해서, 응답을 기다리지 않고 요청을 연속적으로 보내는 기능입니다. 이것은 커넥션의 지연를 회피하고자 하는 방법입니다. 이론적으로는, 두 개의 HTTP 요청을 하나의 TCP 메시지 안에 채워서(be packed) 성능을 더 향상시킬 수 있습니다. HTTP 요청의 사이즈는 지속적으로 커져왔지만, 일반적인 MSS(최대 세그먼트 크기)는 몇 개의 간단한 요청을 포함하기에는 충분히 여유있습니다.

    > 모든 종류의 HTTP 요청이 파이프라인으로 처리될 수 있는 것은 아니며 (...중략...) 오늘날, 모든 HTTP/1.1 호환 프록시와 서버들은 파이프라이닝을 지원해야 하지만, 실제로는 많은 프록시와 서버들은 제한을 가지고 있습니다. 모던 브라우저가 이 기능을 기본적으로 활성화하지 않는 이유입니다.

  - 파이프라이닝은 더 나은 알고리즘인 멀티플렉싱으로 대체되었는데, 이는 HTTP/2에서 사용됩니다.

## 한계 및 주의 사항

- Keep Alive를 이용한 영속적인 커넥션사용시, 유휴 상태일때에도 서버 리소스를 소비하며, 과부하 상태에서는 DoS attacks을 당할 수 있습니다.

- HTTP 파이프라이닝은 실제로는 많은 프록시와 서버들은 제한을 가지고 있습니다. 그래서 모던 브라우저가 이 기능을 기본적으로 활성화하지 않고 있습니다.(결과적으로 Keep Alive 설정은 비표준이다.)

- [Keep Alive 경고](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Keep-Alive) : 이 기능은 비표준이므로 실제 프로덕션에서 사용하지 마세요. 모든 사용자 환경에서 작동하지 않을 수 도 있으며, 미래에 호환성 문제가 생길 수 있습니다.

## 더 알아볼 것

- (HTTP 1.x를 개선한) HTTP 2에서 추가된 커넥션 관리 모델은 무엇이 있나?
- 3-way Handshake, 4-way HandSahke 작동원리

참고

- [Keep Alive란?](https://etloveguitar.tistory.com/137)

- [Keep Alive와 파이프라이닝 ](https://seonghui.github.io/TIL/docs/http/keep-alive-and-pipelining.html)

- [MDN Keep Alive](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Keep-Alive)

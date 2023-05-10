# SpringBoot와 SpringFramework 차이

**updated 2023.05.10**

## Spring Framework

over view...

> Spring 프레임워크는 모든 종류의 배포 플랫폼에서 최신 Java 기반 엔터프라이즈 애플리케이션을 위한 포괄적인 프로그래밍 및 구성 모델을 제공합니다.

Spring Framework는 말 그대로 어플리케이션 프레임워크로, JSP같은 다양한 프레임워크를 지원하기 때문에
프레임워크의 프레임 워크라고 볼 수 있다.

## SpringBoot

over view...

> Spring Boot를 사용하면 "그냥 실행"할 수 있는 독립 실행형 프로덕션급 Spring 기반 애플리케이션을 쉽게 만들 수 있습니다.
> Spring 플랫폼과 타사 라이브러리에 대한 의견을 반영하여 번거로움을 최소화하면서 시작할 수 있습니다. 대부분의 Spring Boot 애플리케이션은 최소한의 Spring 구성만 필요합니다.

반면 Spring Boot는 Spring + BootStrap.
즉 몇가지 편의를 제공함으로써 좀 더 쉽고 빠르게 앱을 개발 할 수 있는 프레임워크 버전이라고 생각 할 수 있다.

> BootStrap이란?
> 입력 장치에서 나머지 프로그램을 도입할 수 있도록 하는 몇 가지 초기 명령을 통해 컴퓨터에 프로그램을 로드하는 기술입니다

## SpringBoot VS SpringFramework 비교하기

### SpringBoot

- 주요기능을 자동구성하여, 개발 속도를 높이고 단순화 하는데 목적을 둠. 독립실행형 애플리케이션을 개발할 때, 주로 REST API 개발에 사용됨.
- Tomcat, jetty와 같은 내장형 서버를 제공함.
- H2 같은 In-Memory DB 지원
- maven, Gradle 등을 위한 플러그인을 제공
- 개발 및 테스트를 위한 CLI 도구를 제공

### SpringFramework

- 종속성 주입 (Dependency Injection)을 통해, 느슨한 결합을 이루는 애플리케이션을 만드는데 목적을 둠.
- 필요한 종속성은 직접 명시적으로 설정해야 함.
- 별도의 In-Memory DB 지원 없음.
- maven, Gradle 등을 위한 플러그인을 제공하지 않음.
- 개발 및 테스트를 위한 CLI 도구를 제공하지 않음.

## 결론

비교를 통해 알아본 결과, SpringFramework는 순정이고, SpringBoot는 빠른 개발을 위해 기능 추가한 튜닝 버전이다.

처음 SpringFramework를 배우고 입문하기에는 SpringBoot가 도움이 될 것 같다.

다만 SpringFramework의 구성요소 하나하나 공부하면서 알아가고 싶다면, 시간이 걸리더라도 순정 버전으로 작업하는 것도 좋은 방법이 될 것 같다.

참고자료

- [Spring 공홈](https://spring.io/)
- [Spring vs Spring Boot: Know The Difference](https://www.interviewbit.com/blog/spring-vs-spring-boot/)

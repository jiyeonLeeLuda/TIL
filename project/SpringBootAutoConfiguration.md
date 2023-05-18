# SpringBoot AutoConfiguration

**updated 2023.05.18**

## AutoConfiguration 이란?

Spring Boot 자동 구성은 추가한 jar 종속성을 기반으로 Spring 애플리케이션을 자동으로 구성하려고 시도합니다.

## 사용 방법

@Configuration 클래스에  
@EnableAutoConfiguration 또는 @SpringBootApplication 어노테이션을 추가하여 자동구성을 선택해야 합니다.

원하지 않는 특정 자동 구성 클래스가 적용되는 경우 @EnableAutoConfiguration다음 예제와 같이 exclude 특성을 사용하여 비활성화할 수 있습니다.

```Java
@Configuration
@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
public class MyConfiguration {
}
```

## 참고

- [공식문서 3부. 스프링 부트 사용 - 16장 자동구성](https://docs.spring.io/spring-boot/docs/2.0.x/reference/html/using-boot-auto-configuration.html)

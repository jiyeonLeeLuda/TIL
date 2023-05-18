# SpringBoot AutoConfiguration

**updated 2023.05.18**

## AutoConfiguration 이란?

Spring Boot의 가장 큰 장점 중 하나로, 자동 구성(Auto-configuration)은 개발자가 별다른 설정 없이 Spring Boot 프로젝트를 작성하더라도 Spring의 기능들이 자동으로 구성되어 동작하도록 도와줍니다. 추가한 jar 종속성을 기반으로 Spring 애플리케이션을 자동으로 구성하려고 시도합니다.

자동 구성은 Spring Boot의 핵심 원리 중 하나인 "의미 있는 기본 설정"에 기반하고 있습니다. Spring Boot는 일반적인 프로젝트에서 많이 사용되는 기능들을 사전 정의된 구성 클래스에 묶어두고, 프로젝트에서 해당 기능이 필요한 경우 자동으로 구성될 수 있도록 합니다. 이를 통해 개발자는 복잡한 설정 작업 없이도 Spring의 다양한 기능을 사용할 수 있습니다.

## 자동 구성의 동작 방식

1. Classpath 스캐닝: Spring Boot는 프로젝트의 classpath에서 자동 구성에 사용될 수 있는 구성 클래스들을 스캔합니다. 이러한 구성 클래스들은 @Configuration 애노테이션이 붙어 있습니다.

2. 조건부 설정: 자동 구성은 조건부 설정을 통해 어떤 구성이 적용될지 결정합니다. 예를 들어, 특정 라이브러리가 classpath에 있을 때만 해당 기능이 자동으로 구성되도록 할 수 있습니다.

3. Bean 생성: 자동 구성이 필요한 기능들은 해당 기능을 사용하기 위해 필요한 Bean들을 생성합니다. 이러한 Bean들은 Spring의 IoC(Inversion of Control) 컨테이너에 등록되어 다른 Bean들과 함께 동작합니다.

4. 프로퍼티 설정: Spring Boot는 application.properties 또는 application.yml 파일을 통해 프로퍼티 값을 설정할 수 있습니다. 이를 통해 자동 구성된 기능들의 동작 방식을 세부적으로 제어할 수 있습니다.

5. 자동 구성은 개발자에게 시간과 노력을 절약해주고, 일관된 설정과 동작을 제공해줍니다. 개발자는 필요한 기능들을 수동으로 구성하는 대신, Spring Boot의 자동 구성 기능을 활용하여 빠르게 애플리케이션을 개발할 수 있습니다.

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
- [How Spring Boot AutoConfiguration works?](https://medium.com/empathyco/how-spring-boot-autoconfiguration-works-6e09f911c5ce)

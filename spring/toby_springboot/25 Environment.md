
## 자동 구성 정보의 흐름 정리

````ad-info
title: 자동 구성 정보

```ad-note
title: step 1. 자동 구성 후보 로딩

- AutoConfiguration.imports 형식으로 SpringBoot 가 자동 구성 후보들을 가지고 있다.

- 약 150 가지를 후보로 등록해놓았는데 이 모든 것이 다 Bean 으로 등록되는 것이 아니다.
```

<br>

```ad-note
title: step 2. @Conditional 조건 체크 - Class

- Class 레벨에서 먼저 체크

- Starter, Dependency, Classpath Class 등의 Condition 클래스를 통해 조건을 넣은 것들로 체크한다.
```

<br>

```ad-note
title: step 3. @Conditional 조건 체크 - @Bean 조건 체크

- 클래스레벨의 조건을 검증 한 이후에 그 클래스 내부의 @Bean 을 검증한다.

- 주로 MissingBean 조건이 조합되어 사용된다. [[24 스프링부트의 @Conditional#Bean Conditions]] 참조
```


````



## Environment Properties

@AutoConfiguration 과 @Bean 구성정보를 통해 설정되는 Bean 들은 기본적으로 세부 설정들이 있다.

이러한 default 세부 설정을 변경하는 방법중에 하나가 Environment Properties 를 활용하는 방법



## Environment Abstraction - Properties (Environment 추상화)


![스크린샷 2023-03-01 오후 6 48 20](https://user-images.githubusercontent.com/74750901/222123305-3b271d5b-c16c-45b2-b241-34e0e0fe5130.png)


```ad-info
title: Standard Environment

- System Properties : JAVA 에서 기본적으로 제공하는 프로퍼티. 커맨드라인에서 지정가능

- System Environment Variables : OS 에 환경변수를 미리 세팅하고 사용하는 방법
```

```ad-info
title: Standard Servlet Environment

- WEB 환경일때 사용하는 것들

- ServletConfig Parameters : 서블릿을 사용하면 xml 이나 서블릿 초기화 코드에 지정하여 사용하는 것. ServletConfig 레벨에서 파라미터로 사용

- ServletContext Parameters : ServletContext 레벨에서 파라미터로 사용
 
- JNDI : JAVA 에서 사용하는 네이밍과 디렉토리를 제공하는 표준이지만 잘 사용되지 않음 

```

Standard Environment 와 Standard Servlet Environment 는 Spring 에서 제공되었던 기술들

```ad-info
title: Spring boot 에서 추가로 제공되는 기술

- application.properties, xml, yml : 환경 변수를 제공하기 위한 파일 형식

```


환경 설정 방법들에 우선순위가 있다. 우선순위가 먼저인 것이 먼저 적용이된다. 


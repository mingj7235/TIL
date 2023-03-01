
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



## Environment Abstraction - Properties

-- photo



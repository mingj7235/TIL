
## @Conditional 을 알아야 하는 이유🌱

스프링이 제공하는 자동구성을 알아야할 필요가 있다.
직접 Bean 을 어플리케이션으로 가져오거나, 필요한 경우에 커스텀 Bean 으로 만드는 경우
자동 구성 형태로 내가 만든 Bean 을 등록할 경우에 어떻게 활용하는지를 알아야한다.


```ad-info
title: 스프링 프레임워크의 @Profile 도 @Conditional 어노테이션이다.

~~~java

@Conditional(ProfileCondition.class)
public @interface Profile {

...
}
~~~

- @Conditional 을 메타어노테이션으로 가지고 있다.
```


## Class Conditions

```ad-info

- @ConditionalOnClass
- @ConditionalOnMissingClass
```

- 지정한 클래스의 프로젝트내 존재를 확인해서 포함 여부를 결정한다.
- 주로 @Configuration 클래스 레벨에서 사용하지만 @Bean 메소드에도 적용 가능하다. 
- 단, 클래스 레벨의 검증없이 @Bean 메소드에만 적용하면 불필요하게 @Configuration 클래스가 빈으로 등록되기 때문에, 클래스 레벨 사용을 우선해야한다.


## Bean Conditions

```ad-info

- @ConditionalOnBean
- @ConditionalOnMissingBean
```

- 빈의 존재 여부를 기준으로 포함여부를 결정한다. 빈의 타입 또는 이름을 지정할 수 있다.
- 지정한 빈 정보가 없으면 메소드의 **리턴 타입을 기준**으로 존재여부를 체크한다.
- 스프링 컨테이너에 등록된 빈 정보를 기준으로 체크하기 때문에 자동 구성 사이에 적용하려면 @Configuration **클래스의 적용 순서**가 중요하다.
	- **개발자가 직접 정의한 커스텀 빈 구성 정보가 자동 구성 정보 처리보다 우선**하기 때문에 이 관계에 적용하는 것은 안전하다.
	- 커스텀 빈 구성정보에 적용하는 것은 피해야 한다.

```ad-note
- @Configuration 클래스 레벨의 @ConditionalOnClass 와 @Bean 메소드 레벨의 @ConditionalOnMissingBean 조합은 가장 대표적으로 사용되는 방식이다.
- [[23 자동 구성 정보 대체하기]] 예제에서도 사용했다.
- 클래스의 존재로 해당 기술의 사용 여부를 확인하고, 직접 추가한 커스텀 빈 구성의 존재를 확인 하여 자동 구성의 빈 오브젝트를 이용할지 최종적으로 결정하는 것이다.
```


## Property Conditions

```ad-info
- @ConditionalOnProperty
```

- 스프링의 환경 프로퍼티 정보를 이용한다.
- 지정된 프로퍼티가 존재하고 값이 false 가 아니면 포함되상이 된다.
- 특정 값을 가진 경우를 확인하거나 프로퍼티가 존재하지 않을 때 조건을 만족하게 할 수도 있다.
- 프로퍼티의 존재를 확인해서 빈 오브젝트를 추가하고, 해당 빈 오브젝트에서 프로퍼티 값을 이용하여 세밀하게 빈 구성도 가능하다.


## Resource Conditions

```ad-info
- @ConditionalOnResource 
```

- 지정된 리소스(파일)의 존재를 확인하는 조건


## Web Application Conditions

```ad-info
- @ConditionalOnWebApplication
- @ConditionalOnNotWebApplication
```

- 웹 어플리케이션 여부를 확인한다. (모든 스프링 부트 프로젝트가 웹 기술을 사용해야 하는 것은 아니므로..!)
	- Batch 서버등을 구성할 때 사용 가능


## SpEL Expression Condtions

```ad-info
- @ConditionalOnExpression
```

- 스프링 SpEL 의 처리 결과를 기준으로 판단한다.
- 매우 복잡하면서도 세밀한 조건 설정이 가능하다.



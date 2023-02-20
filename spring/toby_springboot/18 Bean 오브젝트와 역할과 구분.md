
Spring Container 가 관리하는 Bean 은 크게 '컨테이너 인프라스트럭처 빈' 과 '어플리케이션 빈' 으로 구분된다.



## 컨테이너 인프라스트럭처 빈 (Container Infrastructure Bean)
- Spring Container 자신이거나, Spring Container 가 계속해서 기능을 확장하면서 추가해온 것들을 Bean 으로 등록해서 사용한 것들.
- Spring Container 가 스스로 Bean 으로 등록한 것들
- Spring Container 가 기능을 하기 위해서 존재해야하는 Bean 들. 
- 일반적으로 개발자들의 관심사가 아니다. 또한 어플리케이션에서 이것들을 직접 참조해서 사용하는 일도 없다. (물론, 당연히 원한다면 DI 하여 참조 가능하다.)
- 


## 어플리케이션 빈 (Application Bean)

- 개발자가 어떤 Bean 을 사용하겠다고 명시적으로 구성정보를 제공한 것을 의미한다.
- 그 제공된 구성정보(Configuration MetaData) 를 바탕으로 Spring Container 가 Bean으로 등록한 것들. 

- 어플리케이션 빈도 두가지로 구분이 가능하다.
	- 1. 어플리케이션 로직 빈 (Application Logic Bean)
		- 어플리케이션의 비지니스 로직, 도메인 로직을 담고 있는 Bean 을 의미함.
		- 즉, 개발자가 개발한다고 흔히 하는 코드들.
		- 즉, 사용자 구성정보를 이용해서 등록하는 Bean -> ComponentScan 을 이용해서 자동으로 Java 코드와 어노테이션으로 구성정보를 읽어온다. 
			- @ComponentScan 을 통해 Bean 이 등록되는 것!
		- ex> HelloController, SimpleHelloService
		
	- 2. 어플리케이션 인프라스트럭쳐 빈 (Application Infrastructure Bean)
		- 대부분 '기술'과 관련된 것.
		- 개발자가 직접 개발하지 않고, 이 어플리케이션에서는 이런 이런 기술을 사용하겠다 라고 명시적으로 Spring Container 에 구성정보를 제공하는 것.
		- 구성정보를 제공하지 않으면 Bean 으로 등록 되지 않는다.
		- 즉, 자동 구성 정보를 이용해서 등록하는 Bean -> AutoConfiguration 을 이용해서 구성정보를 읽어온다. 
			- @AutoConfiguration 을 통해 Bean 이 등록되는 것!
		- ex> DataSource, JpaEntityManagerFactory, JdbcTransactionManaber


## TomcatServletWebServerFactory 와 DispatcherServlet 은?

- 이 두가지는 어디에 속할까?
- 컨테이너 인프라스트럭처 빈이라고 생각 할 수 있지만 그렇지 않다. 
	- 명시적으로 선언을 해줘야지만 Bean 으로 등록이 되기 때문이다.
- 그렇기에, 이 두 클래스는 어플리케이션 인프라스트럭쳐 빈 으로 분리되는 것이 맞다.

- @Configuration 클래스 내부에서 @Bean 을 통해 등록되는 어플리케이션 인프라스트럭쳐 빈은 AutoConfiguration 이라는 자동 구성 정보 를 통해 Bean 으로 읽혀진다.


## 인프라 빈 구성정보의 분리

TomcatServletWebServerFactory 즉, Servlet Container 를 만드는 것과
DispatcherServlet 즉, Dispatcher Servlet 을 만드는 것은 분리가 되어야한다.

개발자가 어플리케이션을 만드는 것과는 관심사가 분리 되어야한다.

@ComponentScan 이 붙은 main 메소드가 있는 클래스와 같은 패키지나 하위 패키지에 있다면, 
@Configuration 또한 @Component 이므로 Component Scanner 로 인해 스캔 대상이 되어 Bean 등록이 될 것이다.
하지만, 이렇게 된다면 하위 패키지에 존재해야하며, 개발자의 관심사에 들어가게 되어 관리 대상이 된다.

그러므로, 분리가 되어야 한다. 분리가 되어서도 구성정보가 어플리케이션이 시작되면서 Bean으로 등록이 되어야 한다. 

어떻게 하면될까?

비밀은 바로 @Import({추가하고 싶은 어노테이션.class}) 어노테이션 이다.
@Import() 의 인자값에 @Component 이나, @Component 를 메타 어노테이션으로 붙은 클래스를 넣으면, 구성정보에 직접 추가할 수 있다. 

```java
@Retention(RetentionPolicy.RUNTIME)  
@Target(ElementType.TYPE)  
@Configuration  
@ComponentScan  
@Import(Config.class)  
public @interface MySpringBootApplication {    
}

------------------------------------------ 

@Configuration  
public class Config {  
    @Bean  
    public ServletWebServerFactory servletWebServerFactory () {  
        return new TomcatServletWebServerFactory();  
    }  
    @Bean  
    public DispatcherServlet dispatcherServlet () {  
        return new DispatcherServlet();  
    }  
}

```

### 인프라 빈 구성정보 Refactor

- Config 클래스가 가지고 있던 두개의 Bean 을 분리

```java
@Configuration  
public class TomcatWebServerConfiguration {  
    @Bean  
    public ServletWebServerFactory servletWebServerFactory () {  
        return new TomcatServletWebServerFactory();  
    }  
}

----------------------------------------------------

@Configuration  
public class DispatcherServletConfig {  
    @Bean  
    public DispatcherServlet dispatcherServlet () {  
        return new DispatcherServlet();  
    }  
  
}

```

- @MySpringBootApplication 에서 @Import 관심사를 다른 클래스로 분리

```java
@Retention(RetentionPolicy.RUNTIME)  
@Target(ElementType.TYPE)  
@Import({DispatcherServletConfig.class, TomcatWebServerConfiguration.class})  
public @interface EnableMyAutoConfiguration {  
  
}
```

- 최종 @MySpringBootApplication

```java
@Retention(RetentionPolicy.RUNTIME)  
@Target(ElementType.TYPE)  
@Configuration  
@ComponentScan  
@EnableMyAutoConfiguration  
public @interface MySpringBootApplication {  
  
}
```


@MySpringBootApplication 은 @Configuration 과 @ComponentScan 의 Composed Annotation 이며, @EnableMyAutoConfiguration 을 메타 어노테이션으로 가지고 있다.

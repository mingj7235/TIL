
## 코드

```java
@Retention(RetentionPolicy.RUNTIME)  
@Target(ElementType.TYPE)  
@Configuration(proxyBeanMethods = false)  
public @interface MyAutoConfiguration {  
}
```

- @Configuration 에서 proxyBeanMethods 를 false 로 주었다. (default 는 true)

```java
@MyAutoConfiguration  
public class DispatcherServletConfig {  
    @Bean  
    public DispatcherServlet dispatcherServlet () {  
        return new DispatcherServlet();  
    }   
}

---------------------------------------------------------
@MyAutoConfiguration  
public class TomcatWebServerConfiguration {  
    @Bean  
    public ServletWebServerFactory servletWebServerFactory () {  
        return new TomcatServletWebServerFactory();  
    }  
}
```

- 구성정보로 들어오는 두 클래스의 어노테이션을 @Configuration 에서 @MyAutoConfiguration 으로 변경
	- MyAutoConfiguration 이 Configuration 을 메타 어노테이션으로 가지고 있으므로 가능
	- 바꾸지 않아도 동작하지만, 구성정보 Bean 에는 @AutoConfiguration 을 붙여주는 것이 관례기 때문.


## @Configuration 이 붙은 클래스의 특별한 동작 방식

@Configuration 은 특별하다!

- 학습 테스트 (기술 이해와 학습을 위해 만든 테스트)
```
```

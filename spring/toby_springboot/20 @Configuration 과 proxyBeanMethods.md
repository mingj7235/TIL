
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

학습 테스트 (기술 이해와 학습을 위해 만든 테스트) 코드

- 테스트를 위해 사전작업

```java
@Configuration  
static class MyConfig {  
    @Bean  
    Common common () {  
        return new Common();  
    }  
  
    @Bean  
    Bean1 bean1() {  
        return new Bean1(common());  
    }  
  
    @Bean  
    Bean2 bean2() {  
        return new Bean2(common());  
    }  
}  
  
static class Bean1 {  
    private final Common common;  
    Bean1(final Common common) {  
        this.common = common;  
    }  
}  
  
static class Bean2 {  
    private final Common common;  
    Bean2(final Common common) {  
        this.common = common;  
    }  
}  
  
static class Common {  
  
}
```
- Common 이라는 클래스를 Bean1 과 Bean2 가 생성자로 의존하고 있다.


````ad-note 
title: Test1

~~~java
@Test  
void configuration () {  
  
    MyConfig myConfig = new MyConfig();  
    Bean1 bean1 = myConfig.bean1();  
    Bean2 bean2 = myConfig.bean2();  
  
    Assertions.assertThat(bean1.common).isSameAs(bean2.common);  
  
}
~~~~

<br>

```ad-fail
title: 테스트가 실패한다.

당연히 Java 에서는 bean1.common 과 bean2.common 은 다른 주소값을 갖기 때문이다.
```

````



````ad-note 
title: Test2

~~~java
@Test  
void configuration () {  
  
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext();  
    ac.register(MyConfig.class);  
    ac.refresh();  
  
    Bean1 bean1 = ac.getBean(Bean1.class);  
    Bean2 bean2 = ac.getBean(Bean2.class);  
  
    Assertions.assertThat(bean1.common).isSameAs(bean2.common);  
  
}
~~~~

<br>

```ad-success
title: 테스트가 성공한다.

- 이것이 @Configuration 의 비밀
- @Configuration 이 ProxyBeanMethod 가 Default로 true 값이고, MyConfig.class 가 Bean 으로 등록될 때, 직접 등록되지 않고 Proxy 로 등록이 된다.

```

````


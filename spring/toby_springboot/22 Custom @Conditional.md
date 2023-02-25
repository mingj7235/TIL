## @Conditional 을 활용한 자동구성

````ad-note
title: 1. 의존 라이브러리에 따른 Conditional 구성

~~~java
@MyAutoConfiguration  
@Conditional(JettyWebServerConfiguration.JettyCondition.class)  
public class JettyWebServerConfiguration {  
    @Bean("jettyWebServerFactory")  
    public ServletWebServerFactory servletWebServerFactory () {  
        return new JettyServletWebServerFactory();  
    }  
  
    static class JettyCondition implements Condition {  
  
        @Override  
        public boolean matches(final ConditionContext context, final AnnotatedTypeMetadata metadata) {  
            return ClassUtils.isPresent("org.eclipse.jetty.server.Server",  
                    context.getClassLoader());  
        }  
  
    }  
}
~~~


<br>

```ad-info
title: 해당 어플리케이션이 특정 라이브러리를 의존하여 받은 클래스가 존재하는가.

- build.gradle 을 통해 의존 라이브러리를 체크하고, 해당 조건에 따라 @Conditional 을 통해 Bean 을 등록한다. 
```
````


## Custom @Conditional 을 활용

````ad-note
title: Custom 한 @Conditional 을 생성

<br>

```ad-info
title: 커스텀 어노테이션인 - ConditionalMyOnclass 

~~~java
@Retention(RetentionPolicy.RUNTIME)  
@Target({ElementType.TYPE, ElementType.METHOD})  
@Conditional(MyOnClassCondition.class)  
public @interface ConditionalMyOnClass {  
    String value();  
}
~~~
```

<br>

```ad-info
title: Condition 인터페이스를 구현한 MyOnClassCondition 클래스

~~~java
public class MyOnClassCondition implements Condition {  
    @Override  
    public boolean matches(final ConditionContext context, final AnnotatedTypeMetadata metadata) {  
        Map<String, Object> attributes = metadata.getAnnotationAttributes(ConditionalMyOnClass.class.getName());  
        String value = (String) attributes.get("value");  
        return ClassUtils.isPresent(value, context.getClassLoader());  
    }  
  
}
~~~
```
<br>

```ad-info
title: TomcatWebServerConfiguration

~~~java
@MyAutoConfiguration  
@ConditionalMyOnClass("org.apache.catalina.startup.Tomcat")  
public class TomcatWebServerConfiguration {  
    @Bean("tomcatWebServerFactory")  
    public ServletWebServerFactory servletWebServerFactory () {  
        return new TomcatServletWebServerFactory();  
    }  
}
~~~
```
<br>


```ad-info
title: JettyWebServerConfiguration

~~~java
@MyAutoConfiguration  
@ConditionalMyOnClass("org.eclipse.jetty.server.Server")  
public class JettyWebServerConfiguration {  
    @Bean("jettyWebServerFactory")  
    public ServletWebServerFactory servletWebServerFactory () {  
        return new JettyServletWebServerFactory();  
    }  
}
~~~
```

- [[21 조건부 자동 구성#학습 테스트 코드]] 에서 했듯이 Condition 클래스를 생성하여 어노테이션의 attributes 를 통해 @Conditional 을 설정한다. 

- 어플리케이션이 어떤 ServletContainer 를 의존성으로 가지고 있느냐에 따라 구성정보를 동적으로 가져올 수 있다.

````



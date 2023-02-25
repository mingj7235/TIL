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


````



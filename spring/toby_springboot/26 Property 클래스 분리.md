
````ad-note
title: Property 클래스 분리

<br>

```ad-info
title: application.properties

~~~properties
contextPath=/app  
port=8080
~~~

- 해당 정보를 어플리케이션에 어떻게 적용시키는가? 
```

<br>

```ad-info
title: ServerProperties

~~~java
public class ServerProperties {  
  
    private String contextPath;  
  
    private int port;  
  
    public String getContextPath() {  
        return contextPath;  
    }  
  
    public void setContextPath(final String contextPath) {  
        this.contextPath = contextPath;  
    }  
  
    public int getPort() {  
        return port;  
    }  
  
    public void setPort(final int port) {  
        this.port = port;  
    }  
  
}
~~~

- application.properties 에 담겨있는 설정 정보 키값과 동일한 필드네임으로 ServerProperties 클래스 생성

```


<br>

```ad-info
title:

~~~java
@MyAutoConfiguration  
public class ServerPropertiesConfig {  
  
    @Bean  
    public ServerProperties serverProperties(Environment env) {  
        ServerProperties serverProperties = new ServerProperties();  
        serverProperties.setContextPath(env.getProperty("contextPath"));  
        serverProperties.setPort(Integer.parseInt(Objects.requireNonNull(env.getProperty("port"))));  
    }  
  
}
~~~

- 위의 코드는 Binder 를 사용하여 아래와 같이 개선될 수 있다.

<br>

~~~java
@MyAutoConfiguration  
public class ServerPropertiesConfig {  
  
    @Bean  
    public ServerProperties serverProperties(Environment env) {  
        return Binder.get(env).bind("", ServerProperties.class).get();  
    }  
}
~~~

- Binder 클래스를 통해, ServerProperties 클래스 네임과 메핑이되는 property 정보를 가져온다.
```

<br>


```ad-info
title: TomcatWebServerConfiguration

~~~java
@MyAutoConfiguration  
@ConditionalMyOnClass("org.apache.catalina.startup.Tomcat")  
public class TomcatWebServerConfiguration {  
  
    @Bean("tomcatWebServerFactory")  
    @ConditionalOnMissingBean  
    public ServletWebServerFactory servletWebServerFactory (ServerProperties properties) {  
        TomcatServletWebServerFactory tomcatServletWebServerFactory = new TomcatServletWebServerFactory();  
        tomcatServletWebServerFactory.setContextPath(properties.getContextPath());  
        tomcatServletWebServerFactory.setPort(properties.getPort());  
        return tomcatServletWebServerFactory;  
    }  
}
~~~

- TomcatServlet 의 ContextPath와 Port 를 ServerProperties 라는 클래스를 통해 주입
```

<br>



````

## MySpringApplication.run()

```java

public class MySpringApplication {  
    public static void run(Class<?> applicationClass, String[] args) {  
        AnnotationConfigWebApplicationContext applicationContext = new AnnotationConfigWebApplicationContext() {  
            @Override  
            protected void onRefresh() {  
                super.onRefresh();  
  
                ServletWebServerFactory serverFactory = this.getBean(ServletWebServerFactory.class);  
                DispatcherServlet dispatcherServlet = this.getBean(DispatcherServlet.class);  
  
                WebServer webServer = serverFactory.getWebServer(servletContext -> {  
                    servletContext.addServlet("dispatcherServlet", dispatcherServlet)  
                            .addMapping("/*");  
                });  
                webServer.start();  
            }  
        };  
        applicationContext.register(applicationClass);  
        applicationContext.refresh();  
    }  
  
}

----------------------------------------------------

@Configuration  
@ComponentScan  
public class Application {  
   @Bean  
   public ServletWebServerFactory servletWebServerFactory () {  
      return new TomcatServletWebServerFactory();  
   }  
  
   @Bean  
   public DispatcherServlet dispatcherServlet () {  
      return new DispatcherServlet();  
   }  
  
   public static void main(String[] args) {  
      MySpringApplication.run(Application.class, args);  
   }   
}

```

- 지금까지 한 과정은, 처음 Spring Boot Application 을 만들었을 때, main 메소드에 한 줄 있었던 **SpringApplication.run(Application.class, args);** 가 어떻게 만들어졌는지 만들어 본 것.
	- 물론, Application 클래스에 있는 세부적인 내용 (팩토리 메소드, 어노테이션) 은 차이가 있으나, 어떻게 run 메소드 하나로 서블릿 컨테이너와 디스페쳐 서블릿이 생성되고 등록되어 Spring Application 이 되는지 과정을 본것


## SpringApplication.run()

```java
@Configuration  
@ComponentScan  
public class Application {  
   @Bean  
   public ServletWebServerFactory servletWebServerFactory () {  
      return new TomcatServletWebServerFactory();  
   }  
   @Bean  
   public DispatcherServlet dispatcherServlet () {  
      return new DispatcherServlet();  
   }  
   public static void main(String[] args) {  
      SpringApplication.run(Application.class, args);  
   }  
}
```

- 익숙한 SpringApplication.run()
- 여기서 위의 펙토리 메소드를 이제 더 이상 등록하지 않아도 되는거 아닐까?
	- 아니다, 위의 펙토리 메소드를 지우고 서버를 실행시키면, bean 을 찾을 수 없다고 에러가 발생한다. 


### servletWebServerFactory 를 주석할 경우

```java

Description:

Web application could not be started as there was no org.springframework.boot.web.servlet.server.ServletWebServerFactory bean defined in the context.

```

서버자체가 뜨지 않는다. ServletWebServerFactory bean을 찾지 못한다.

### dispatcherServlet 을 주석할 경우

서버는 실행되지만, 요청을 보낼 시, 404 에러가 발생한다. 


### 그럼 왜 처음은 된거지? ㅎ..

**비밀은 바로 @SpringBootApplication 어노테이션.** 

그비밀은 차차 알아가보자 
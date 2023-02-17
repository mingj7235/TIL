
### 코드

```java

@Configuration  
public class Application {  
   @Bean  
   public HelloController helloController (HelloService helloService) {  
      return new HelloController(helloService);  
   }  
   @Bean  
   public HelloService helloService () {  
      return new SimpleHelloService();  
   }  
  
   public static void main(String[] args) {  
      AnnotationConfigWebApplicationContext applicationContext = new AnnotationConfigWebApplicationContext() {  
         @Override  
         protected void onRefresh() { // onRefresh 는 Spring Container 가 초기화 되는 도중에 진행되도록 하는 메소드다.  
            super.onRefresh();  
            // Spring Container 가 초기화 될 때, Dispatcher Servlet 을 생성하도록 하는 것.  
            ServletWebServerFactory serverFactory = new TomcatServletWebServerFactory();  
            WebServer webServer = serverFactory.getWebServer(servletContext -> {  
               servletContext.addServlet("dispatcherServlet",  
                     new DispatcherServlet(this)  
               ).addMapping("/*");  
            });  
            webServer.start();  
         }  
      };  
      applicationContext.register(Application.class);  
      applicationContext.refresh();  
   }  
}
```

- @Configuration : Bean 들이 등록되어있다고 Spring Container 에게 알림. 
- @Bean : Bean 으로 등록하여 사용

- 하.지.만. 위의 방법, 즉 Java Code 를 사용하여 직접 Bean 을 등록하고, Spring Container 에게 전달 하는 방법은 사용하지 않는다.
- 바로 다음에 배우게 될 @ComponentScan 을 통해 
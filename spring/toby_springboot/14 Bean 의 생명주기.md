
## Servlet Container 와 Dispatcher Servlet 을 Bean 으로 등록

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
      AnnotationConfigWebApplicationContext applicationContext = new AnnotationConfigWebApplicationContext() {  
         @Override  
         protected void onRefresh() {  
            super.onRefresh();  
            ServletWebServerFactory serverFactory = this.getBean(ServletWebServerFactory.class);  
            DispatcherServlet dispatcherServlet = this.getBean(DispatcherServlet.class);  
            //dispatcherServlet.setApplicationContext(this);  
  
            WebServer webServer = serverFactory.getWebServer(servletContext -> {  
               servletContext.addServlet("dispatcherServlet", dispatcherServlet)  
                     .addMapping("/*");  
            });  
            webServer.start();  
         }  
      };  
      applicationContext.register(Application.class);  
      applicationContext.refresh();  
   }  
}
```


1. 팩토리 메소드를 통해서 Servlet Container 와 DispatcherServlet 을 Bean 으로 등록  
2. Spring Container 초기화를 할 때, getBean() 을 통해 1.에서 등록한 Servlet Container 와 Dispatcher Servlet Bean 을 가져옴  
3. dispatcherServlet 의 setApplicationContext() 메소드를 통해 this, 즉 Spring Container 를 등록해준다.


- 그런데 여기에서 3번 즉, Spring Container 을 DispatcherServlet 에 등록하지 않아도 동작한다. 
```java
dispatcherServlet.setApplicationContext(this); 삭제 
```

어떻게 이런 일이 발생하는가?

-> Spring Container 가 DispatcherServlet 에 주입을 해준 것 !
-> Bean 의 '라이프사이클 메소드' 개념을 알아야함
-> ApplicaitonContextAware 에서 setApplicationContext 를 통해 주입을 해준다.


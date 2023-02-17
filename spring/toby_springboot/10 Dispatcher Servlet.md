
### FrontController Servlet -> Dispatcher Servlet 

```java

public static void main(String[] args) {  
   GenericWebApplicationContext applicationContext = new GenericWebApplicationContext();  
   applicationContext.registerBean(HelloController.class);  
   applicationContext.registerBean(SimpleHelloService.class);  
   applicationContext.refresh();  
  
   ServletWebServerFactory serverFactory = new TomcatServletWebServerFactory();  
   WebServer webServer = serverFactory.getWebServer(servletContext -> {  
      servletContext.addServlet("dispatcherServlet", new DispatcherServlet(applicationContext)
            ).addMapping("/*");  
   });  
   webServer.start();  
}
```

- FrontController 가 담당했던 부분을 대신 해주는 Dispatcher Servlet.
- Spring Controller와 연결 하기 위해 DispathcerServlet 생성자로 GenericWebApplicationContext 객체를 넘겨주었다.
- 아직 매핑 정보는 넣지 않은 상태
- 여기서 어노테이션 매핑 정보를 사용하여 DispatcherServlet 은 요청과 컨트롤러를 매핑한다.




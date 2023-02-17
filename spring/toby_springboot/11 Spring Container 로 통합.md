### Spring Container 가 초기화 되는 중에 Dispatcher Servlet 을 생성 하도록 

```java
public static void main(String[] args) {  
   GenericWebApplicationContext applicationContext = new GenericWebApplicationContext() {  
      @Override  
      protected void onRefresh() {
         super.onRefresh();  
         ServletWebServerFactory serverFactory = new TomcatServletWebServerFactory();  
         WebServer webServer = serverFactory.getWebServer(servletContext -> {  
            servletContext.addServlet("dispatcherServlet",  
                  new DispatcherServlet(this)  
            ).addMapping("/*");  
         });  
         webServer.start();  
      }  
   };  
   applicationContext.registerBean(HelloController.class);  
   applicationContext.registerBean(SimpleHelloService.class);  
   applicationContext.refresh();  
}
```

- onRefresh() 는 Spring Container 가 초기화 되는 중에 특정한 로직을 수행하도록 하는 메소드.
	- 해당 메소드를 실행하기 위해 익명클래스를 만들고 override 하여 그 안에 Dispatcher Servlet 을 생성하도록 하였다. 

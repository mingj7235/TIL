
### Servlet

- servlet 은 각각 url 에 매핑되도록 여러개의 servlet 을 등록해야한다.


```java
// Servlet

ServletWebServerFactory serverFactory = new TomcatServletWebServerFactory();  
      WebServer webServer = serverFactory.getWebServer(servletContext -> {  
         servletContext.addServlet("hello", new HttpServlet() {  
            @Override  
            protected void service(final HttpServletRequest req, final HttpServletResponse resp) throws ServletException, IOException {  
               String name = req.getParameter("name");  
  
               resp.setStatus(HttpStatus.OK.value());  
               resp.setHeader(HttpHeaders.CONTENT_TYPE, MediaType.TEXT_PLAIN_VALUE);  
               resp.getWriter().println("Hello " + name);  
            }  
         }).addMapping("/hello");  
      });  
      webServer.start();  

```

### Front Controller 의 등장

- Servlet 이 여러개가 만들어지고, 모든 요청을 Servlet Container 로부터  각각의 Servlet 에 매핑시키다보니, Servlet 들이 작업을 수행할 때, 공통적으로 하는 '중복적인' 코드가 나타나는 것을 보고 이것을 개선시킬 방안을 위해 고안

- 기본적인 Servlet 웹 요청과 응답을 req, resp object 를 다뤄야하기 때문에, 기능에 있어서 한계가 있었다.

- Front Controller 는 모든 Servlet 들의 공통적인 코드들을 가장 앞단에서 처리 하게 하고, 각 요청에 따라 로직을 위임하도록 하는 것. 즉, 공통적인 부분을 앞단에서 처리하도록 하는것. (물론 후처리에도 공통적인 내용이 있다면 이것들도 한번에 함)
	- 인증, 보안, 다국어처리 등

- Front Controller 를 활용한 어플리케이션 개발이 대표적인 패턴으로 자리 잡게 되었다.

```java
// Front Controller

ServletWebServerFactory serverFactory = new TomcatServletWebServerFactory();  
WebServer webServer = serverFactory.getWebServer(servletContext -> {  
   servletContext.addServlet("frontController", new HttpServlet() {  
      @Override  
      protected void service(final HttpServletRequest req, final HttpServletResponse resp) throws ServletException, IOException {  
         // 인증, 보안, 다국어처리, 각종 공통 기능 (구현했다고 치고)  
  
         if (req.getRequestURI().equals("/hello") &&  req.getMethod().equals(HttpMethod.GET.name())) {  
            String name = req.getParameter("name");  
  
            resp.setStatus(HttpStatus.OK.value());  
            resp.setHeader(HttpHeaders.CONTENT_TYPE, MediaType.TEXT_PLAIN_VALUE);  
            resp.getWriter().println("Hello " + name);  
         }  
         if (req.getRequestURI().equals("/hello") &&  req.getMethod().equals(HttpMethod.POST.name())) {  
            // post  
         }  
  
         else if (req.getRequestURI().equals("/user")) {  
            // code for /user  
         }  
         else {  
            resp.setStatus(HttpStatus.NOT_FOUND.value());  
         }  
      }  
   }).addMapping("/*");  
});  
webServer.start();

```


### 매핑과 바인딩

위의 코드에서 복잡한 비지니스 로직을 Controller 로 위임

```java
public class HelloController {  
  
    public String hello (String name) {  
	    // 복잡한 비지니스 로직이라고 치자 ㅎㅎ.. 
        return "Hello " + name;  
    }  
}
```


```java
ServletWebServerFactory serverFactory = new TomcatServletWebServerFactory();  
WebServer webServer = serverFactory.getWebServer(servletContext -> {  
  
   HelloController helloController = new HelloController();  
  
   servletContext.addServlet("frontController", new HttpServlet() {  
      @Override  
      protected void service(final HttpServletRequest req, final HttpServletResponse resp) throws ServletException, IOException {  
         // 인증, 보안, 다국어처리, 각종 공통 기능 (구현했다고 치고)  
  
         if (req.getRequestURI().equals("/hello") &&  req.getMethod().equals(HttpMethod.GET.name())) {  
            String name = req.getParameter("name");  
  
            String result = helloController.hello(name); // 복잡한 비지니스 로직은 여기에 위임  
  
            resp.setStatus(HttpStatus.OK.value());  
            resp.setHeader(HttpHeaders.CONTENT_TYPE, MediaType.TEXT_PLAIN_VALUE);  
            resp.getWriter().println(result);  
         }  
         if (req.getRequestURI().equals("/hello") &&  req.getMethod().equals(HttpMethod.POST.name())) {  
            // post  
         }  
  
         else if (req.getRequestURI().equals("/user")) {  
            // code for /user  
         }  
         else {  
            resp.setStatus(HttpStatus.NOT_FOUND.value());  
         }  
      }  
   }).addMapping("/*");  
});  
webServer.start();
```

- 이 모든 과정은 스프링을 사용하지 않고 순수 자바코드와 서블릿 웹 기술만을 사용하여 동작시키도록 만들고 있음

매핑 : 웹 요청에 들어있는 정보를 활용하여 어떤 로직을 수행하는 코드를 호출할 것인지를 결정하는 작업
	-> '/hello' 라는 요청 정보를 활용하여 helloController 에 있는 로직을 수행하도록 결정함

바인딩 : 웹 요청과 응답을 다루는 오브젝트를 컨트롤러에 놓지않고, 파라미터 값으로 넘어온 정보를 순수 자바 메소드의 인자값으로 넘겨주는 행위. 즉, 로직이 올바르게 수행 될 수 있도록, 웹에서 넘어온 요청값을 자바 메소드가 이해할 수 있는 타입으로 변형하여 넘겨 주는 행위. (DTO 를 사용하거나.. 우리가 당연하게 사용하고 있는 행위들의 근본)



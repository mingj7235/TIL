
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
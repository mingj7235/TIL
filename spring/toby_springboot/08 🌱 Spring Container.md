### 독립 실행형 스프링 애플리케이션

- FrontController 를 통해 매핑되었던 HelloController 를 Spring Container 안으로 넣고, Bean 으로 관리 해보자 !

- FrontController 를 직접 우리는 구현했지만...


### Spring Container 은 어떻게 어플리케이션을 만드는가? 

1. POJO (어플리케이션 코드) - 비지니스 로직을 담고 있는 평범한 자바 오브젝트. 
	- 비지니스 로직을 담고 있는 코드라고 생각하면된다.

2.  Configuration Metadata - 위의 비지니스 로직들을 어떻게 구성할 것인지에 대한 그 구성정보를 가지고 있는 데이터. 어플리케이션을 어떻게 구성할 것인가.


이 두가지를 Spring Container 가 조합하여 사용가능한 완전히 구성된 시스템 (Fully Configured System) 을 구성한다.

-- photo

Spring Container 가 내부의 Bean 들을 의 조합을 통해 POJO 와 Configuration Metadata 와 함께 어플리케이션을 만드는 것.

### Spring Controller 를 생성하고, Bean 을 등록 후, ServletContainer 에서 사용

```java
public static void main(String[] args) {  
   GenericApplicationContext applicationContext = new GenericApplicationContext(); // Spring Container  
   applicationContext.registerBean(HelloController.class); // Bean 등록  
   applicationContext.refresh(); // Spring Container 가 본인이 가지고 있는 Configuration Metadata 를 통해 컨테이너를 초기화 하는 작업. -> Bean Object 를 만들어준다.  
  
   ServletWebServerFactory serverFactory = new TomcatServletWebServerFactory();  
   WebServer webServer = serverFactory.getWebServer(servletContext -> {  
  
      servletContext.addServlet("frontController", new HttpServlet() {  
         @Override  
         protected void service(final HttpServletRequest req, final HttpServletResponse resp) throws ServletException, IOException {  
            // 인증, 보안, 다국어처리, 각종 공통 기능 (구현했다고 치고)  
            if (req.getRequestURI().equals("/hello") &&  req.getMethod().equals(HttpMethod.GET.name())) {  
               String name = req.getParameter("name");  
  
               HelloController helloController = applicationContext.getBean(HelloController.class);// 등록한 Bean 을 가져온다. 가져오기만 하면된다.   
String result = helloController.hello(name); // 복잡한 비지니스 로직은 여기에 위임  
  
               resp.setContentType(MediaType.TEXT_PLAIN_VALUE);  
               resp.getWriter().println(result);  
            }  
            else {  
               resp.setStatus(HttpStatus.NOT_FOUND.value());  
            }  
         }  
      }).addMapping("/*");  
   });  
   webServer.start();  
}
```


Spring Container 는 Bean 을 딱 하나만 만든다.
	- 이것이 매우 중요한 것.
	- 싱글톤 패턴으로 Bean 을 만들고 계속 재사용 시킨다.
	- 그렇기에, Spring Container 를 Singlton Registry 라고도 한다. 
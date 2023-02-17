
### FrontController Servlet -> Dispatcher Servlet 

```java

public static void main(String[] args) {
	// 1. Spring Container 생성
   GenericWebApplicationContext applicationContext = new GenericWebApplicationContext();  
   applicationContext.registerBean(HelloController.class);  
   applicationContext.registerBean(SimpleHelloService.class);  
   applicationContext.refresh();  
	
	// 2. DispatchServlet 생성 -> Spring Container 에 등록
   ServletWebServerFactory serverFactory = new TomcatServletWebServerFactory(); // Servlet Container 생성
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

- 위의 코드는 2가지로 분리된다.
	- 1. Spring Container 를 생성하고, Bean 을 등록하여 초기화 하는 부분
	- 2. Servlet Container 를 코드에서 생성하고, Front Controller 역할을 하는 Dispatcher Servlet 을 등록하는 Servlet Container 초기화 하는 부분 



```java
@RequestMapping  
public class HelloController {  
    private final HelloService helloService;  
  
    public HelloController(final HelloService helloService) {  
        this.helloService = helloService;  
    }  
  
    @GetMapping("/hello")  
    @ResponseBody
    public String hello (String name) {  
        return helloService.sayHello(Objects.requireNonNull(name));  
    }  
}
```

- Spring Container 인 applicationContext 를 생성자로 받은 DispatcherServlet 은 Bean 을 모두 다 탐색한다.
	- 이중 에서 웹 요청 정보를 응답할 수 있는 클래스를 찾는다.
	- 그 클래스 중 요청 정보를 처리하는 어노테이션(@GetMapping, @PostMapping,  @RequestMapping 등..)을 가진 녀석들의 URI 를 추출하여 매핑에 사용할 매핑테이블을 만든다. 
	- 그 테이블을 참고하여 이후의 요청이 들어오면, 그 요청을 담당할 Bean Object 와 메소드를 매핑시켜 응답한다.

- Class 단에서 먼저 @RequestMapping 을 참고하고, 그 후에 메소드 레벨에 정보를 추가한다. 
	- DispatcherServlet 은 Class 를 먼저 탐색한다. 
	- Class 단에 @RequestMapping 을 붙이고, 그 뒤에 URI 는 생략해도 된다. 
- String 을 Return 하게 되고 별다른 설정이 없다면, DispatcherServlet 은 View 네임을 리턴하게된다. 
	- 그 view 를 찾지 못하면 404 에러를 뱉는다.
	- String 값을 web body 에 text plain 으로 응답하게 하기 위해서는 @ResponseBody 를 넣어줘야한다.



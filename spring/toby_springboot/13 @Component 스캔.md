
## @Component 과 Component Scanner 이란

스프링 컨테이너에 있는 Component Scanner 이 @Component 가 붙은 클래스들을 모두 찾아서 Bean 으로 등록해준다

@Component : 나를 Bean 으로 등록해줘!!!

```java

@Component  
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

------------------------------------------------------------------

@Component  
public class SimpleHelloService implements HelloService {  
    @Override  
    public String sayHello(String name) {  
        return "Hello " + name;  
    }  
}

------------------------------------------------------------------

@Configuration  
@ComponentScan  
public class Application {  
  
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


@Component
	새로운 빈을 등록할 때, 따로 설정 클래스를 만들고, 그안에 Bean 을 등록하는 것이 아니라,
	등록하고 싶은 클래스에 @Component 어노테이션만 붙이면, Component Scanner 가 Bean 으로 등록해준다. 

@ComponetScan
	Spring Container 에 등록하여 실행되는 Application 클래스 위에 @ComponentScan 을 달아준다.
	이 어노테이션을 붙음으로써, Application 클래스가 존재하는 패키지부터 시작해서 하위 패키지의 모든 클래스중에서 @Component 들을 탐색하고, Bean 으로 등록을 한다. 


## 메타 어노테이션인 @Component

@Component 어노테이션은 메타 어노테이션이다.

메타 어노테이션 : 어노테이션 위에 붙은 어노테이션을 의미한다. 어노테이션 클래스 위에 또 어노테이션을 붙일 수 있다. 

즉, 어떤 어노테이션 위에 @Component 가 붙어있다면, 이 또한 Component Scanner 로 인해 Bean 으로 등록이 된다.

```java

@Retention(RetentionPolicy.RUNTIME) // 언제까지 살아잇을 것인가  
@Target(ElementType.TYPE) // 지정된 타겟 위치  
@Component  
public @interface MyComponent {  
  
}

@MyComponent  
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
- @Component 가 붙은 @MyComponent 를 HelloController 에 달아줘도 정상적으로 동작한다. 


사용하는 이유 
	1. Bean 오브젝트를 계층형으로 만들 때, 어떤 Component 종류인지 구분하기 위함으로 사용할 수 있다 
		- 예) @Controller, @Service, @Repository 등
	2. 
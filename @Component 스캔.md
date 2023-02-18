
### @Component 과 Component Scanner 이란

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



```
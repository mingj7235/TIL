## Autowired

단일 구현체일경우 자동으로 Spring Container 가 주입해주는 것.

1. HelloController 는 Interface 인 HelloService 를 생성자로 주입받는다.
2. HelloService 를 구현한 단일 구현체인 SimpleHelloService 는 @Service 로 인해 Bean 등록을 한다.
3. Spring Container 는 HelloService 의 구현체가 SimpleHelloService 단 하나이므로, 자동으로 DI 해준다.
4. 이를 Autowiring 이라고 한다. 
5. @Autowired 를 통해 어렵지 않게 결정하여 DI 해주었던 것 (springboot 2.3 버전 이후 생략 가능)


## Decorator 패턴

### 사용 이유

>DI에 관한 설명을 보면 대부분 의존하는 오브젝트를 갈아치우는 시나리오를 주로 이야기 한다.
>이것은 전략을 통채로 바꾸는 것. SimpleHelloService 대신 ComplexHelloService로 바꾼다거나 하는 것이다.
>그런데 DI는 오브젝트 합성(composition)을 이용하는 다양한 디자인 패턴을 적용할 때도 유용하다.
>대표적으로 프록시, 데코레이터, 어댑터 등이 있다. 그래서 의존 대상을 바꾸는 경우 외에도 DI가 유용하게 활용되는 예를 들기 위해서 데코레이터를 설명한 것. 
>DI가 없이 데코레이터를 쓰려면 결국 의존하고 있는 쪽에서 구현 클래스 정보를 노출할 수 밖에 없다.
>이걸 DI를 이용해서 코드에서 제거하고 유연한 런타임 의존성 설정으로 가능하게 하는 것을 보여준 것.


### Decorator 사용 의의

> 데코레이터 패턴의 특징은 같은 인터페이스를 구현한 여러개의 클래스가 존재하고 각각이 다음 오브젝트를 DI 받아서 가지고 있다가 호출을 하는 방식이다.
예를 들어 Client -> Service(DefaultService) 두 개의 빈이 있는데 Service 인터페이스를 구현한 기본 클래스가 있는 상태에서 Service 인터페이스를 구현한 데코레이터 D1,D2,D3를 만들었고 적용 순서는 Client -> D1-> D2-> D3-> DefaultService 이렇게 지정을 하고 싶은 경우가 있다.
데코레이터는 동적이어야 하니까 당연히 이 클래스 내부에는 다음에 호출될 클래스 정보를 알고 있으면 안 된다. 대신 구성 정보 어딘가에 이를 넣어줘야 한다.
이 경우 Client는 Service 인터페이스에 의존을 하는데, D1~D3와 DefaultServiec모두 Service인터페이스를 의존하고 있으니까 그 중 D1이 선택되도록 만들려면 특별한 작업이 필요하다.
아주 단순한 방법은 빈의 이름을 이용하는 것. 만약 동일한 타입의 빈이 여러개인 경우 빈의 name이 주입 받을 변수의 이름과 일치하면 그것을 선택해주는 메카니즘이 있다. 
하지만 이렇게 이름을 이용해서 주입을 받게 하면 결국은 클래스 코드가 빈 주입정보를 직접 가지고 있는 셈이 되어서 DI의 의미가 없어진다.
가장 나은 방법은 자바 설정(JavaConfig)을 이용하는 것입니다. 
@Bean 팩토리 메소드에서 D1,D2,D3,DefaultService빈의 구성을 직접 코드로 구현하는 것. 아주 단순하게 만들자면 다음과 같은 식이 된다. 이 경우 DefaultService는 @Component 계열의 빈 설정을 가지고 있으면 안 된다.
```
@Bean  
Service service() {  
return new D1(new D2(new D3(new DefaultService())));  
}
```
>그런데 이렇게 코드로 만들면 결국 DI의 기본인, 코드에 미리 지정되어 있지 않고 동적으로 의존관계가 주입된다는 걸 위반하는 게 아닌가? 근데, 여기서 작성한 @Bean 메소드 코드는 애플리케이션 코드가 아니고 자바 코드로 만든 메타 정보다. 
>따라서 이 경우는 DI가 동작한다고 할 수 있습니다. 그 외에 데코레이터를 AOP를 이용해서 적용하는 방법이 있다. 

### Decorator Simple 코드

```java

public interface HelloService {  
    String sayHello(String name);  
  
}

@Service  
@Primary  
public class HelloDecorator implements HelloService{  
    private final HelloService helloService;  
  
    public HelloDecorator(final HelloService helloService) {  
        this.helloService = helloService;  
    }  
    @Override  
    public String sayHello(final String name) {  
        return "Decorator ** " + helloService.sayHello(name);  
    }  
}

@Service  
public class SimpleHelloService implements HelloService {  
    @Override  
    public String sayHello(String name) {  
        return "Hello " + name;  
    }  
}

@RestController  
public class HelloController {  
    private final HelloService helloService;  
  
    public HelloController(final HelloService helloService) {  
        this.helloService = helloService;  
    }  
  
    @GetMapping("/hello")  
    public String hello (String name) {  
  
        if (name == null || name.trim().length() == 0) throw new IllegalArgumentException();  
  
        return helloService.sayHello(name);  
    }  
}


```

- HelloController -----> HelloDecorater -----> SimpleHelloService

- 최종 응답은, Decorated 된 'Decorator ** Hello + {name}' 이된다

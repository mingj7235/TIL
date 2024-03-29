
### DI 를 하기 위해 Controller, Service 클래스 생성

```java
# HelloController

public class HelloController {  
  
    public String hello (String name) {  
        SimpleHelloService helloService = new SimpleHelloService();  
  
        return helloService.sayHello(Objects.requireNonNull(name));  
    }  
  
}

```

```java
# SimpleHelloService

public class SimpleHelloService {  
  
    String sayHello (String name) {  
        return "Hello " + name;  
    }  
  
}

```

### Spring IoC / DI Container


- Assembler 라는 존재가 필요하다. 소스 코드 단에서의 변경이 아닌, 의존관계를 설정하기 위해 Assembler 의 존재가 필요한 것. 
- 외부에서 객체를 만들고, 주입해주는 것. 이것을 해주는 것이 Assembler 의 역할이다. 
- 직접적인 연관관계가 없는 오브젝트들을 마치 레고 블럭을 조립하는 것처럼, 조립해주는 것.
- 이러한 Assembler 가 바로 Spring Container 다. 

- 주입을 해준다? -> 레퍼런스를 넘겨주는 것
	- HelloController 의 생성자 안에 SimpleHelloService 를 넘기는 것과 같이.. 내가 흔히 아는 생성자 주입의 방식. 
	- 이러한 작업들을 그냥 쓰고 있었지만, 왜 이게 필요한지 아는 것이 중요하다. 


### 강의노트 QnA

> Q : 디자인 패턴이며 oop며 다들 지향하는게 추상화에 의존하라 즉 인터페이스에 의존하는 내용이 많은데요 그 부분을 스프링 빈 사이의 의존성에 연관을 지으니까 조금 의아한 부분이 있더라구요 이 회차 강좌에서 말씀해주신거 처럼 HelloController가 인터페이스(HelloService)를 의존한다고 해도 결국에는 런타임시 SimpleHelloService에 의존적인거죠? 만약 런타임시 CompleHelloService에 의존으로 하려면 결국에는 HelloController 소스를 수정해야하는거죠?

> A: HelloService 인터페이스에만 의존하도록 코드를 작성한다고 했어도, 실제 의존할 오브젝트를 선택하는 것을 직접 해야 한다면 말씀하신 것처럼 HelloController의 코드가 변경이 되야 합니다. 사실상 클래스에도 의존하는 코드가 되는 것이죠.  
바로 이 때문에 어셈블러라는 제3자가 필요한 겁니다. HelloController가 SimpleHelloService 오브젝트를 사용할지, ComplexHelloService를 사용할지를 직접 코드에서 결정하지 않도록, 어셈블러라고 불리는 제3의 오브젝트가 이를 결정해서 생성자에 런타임시에 주입을 해주는 것입니다. 이러면 실제 사용하게 될 오브젝트의 클래스가 바뀌더라도 HelloController의 코드는 전혀 바뀌지 않아도 되는 것이죠.  
의존관계주입(Dependency Injection)이란 바로 이걸 말하는 것입니다. 추상화된 인터페이스만 의존하는 것과 의존관계주입(DI)를 통해서 런타임시에 의존 오브젝트를 주입 받아서 사용하는 것, 이 두가지가 다 필요합니다.
자바도 인터페이스를 사용해서 만드는 코드도 어디선가는 어떤 구현체를 사용할 것이다가 명확하게 등장해야 합니다. List 인터페이스를 쓰는데, 어디선가는 이게 ArrayList라는 구현 클래스다라는 게 나와야 하죠.
스프링도 마찬가지입니다. 다만, 그걸 사용하는 쪽(Controller)가 아닌 스프링 컨테이너의 메타정보로 분리시킬 수 있다는 게 스프링이 하는 일이자 DI의 장점입니다.

### DI 를 활용

```java
# interface 생성 

public interface HelloService {  
    String sayHello(String name);  
  
}

# HelloController 에 interface 주입

public class HelloController {  
    
    private final HelloService helloService;  
  
    public HelloController(final HelloService helloService) {  
        this.helloService = helloService;  
    }  
  
    public String hello (String name) {  
        return helloService.sayHello(Objects.requireNonNull(name));  
    }  
}

# Spring Container 에 SimpleHelloService Bean 등록

public static void main(String[] args) {  
   GenericApplicationContext applicationContext = new GenericApplicationContext(); // Spring Container  
   applicationContext.registerBean(HelloController.class); // Bean 등록  
   applicationContext.registerBean(SimpleHelloService.class);  
   applicationContext.refresh(); // Spring Container 가 본인이 가지고 있는 Configuration Metadata 를 통해 컨테이너를 초기화 하는 작업. -> Bean Object 를 만들어준다.  
  
   ServletWebServerFactory serverFactory = new TomcatServletWebServerFactory();
   ...
   

```

Spring Container 에 SimpleHelloService 를 Bean 으로 등록한다.  

HelloController 에서 생성자를 통해 주입된 인터페이스인 HelloService 를 구현한 Bean 이 있는지 Spring 이 스캔을 하고, SimpleHelloService 의 sayHello 메소드를 수행한다.  
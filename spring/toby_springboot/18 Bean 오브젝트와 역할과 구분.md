
Spring Container 가 관리하는 Bean 은 크게 '컨테이너 인프라스트럭처 빈' 과 '어플리케이션 빈' 으로 구분된다.



## 컨테이너 인프라스트럭처 빈 (Container Infrastructure Bean)
- Spring Container 자신이거나, Spring Container 가 계속해서 기능을 확장하면서 추가해온 것들을 Bean 으로 등록해서 사용한 것들.
- Spring Container 가 스스로 Bean 으로 등록한 것들
- Spring Container 가 기능을 하기 위해서 존재해야하는 Bean 들. 
- 일반적으로 개발자들의 관심사가 아니다. 또한 어플리케이션에서 이것들을 직접 참조해서 사용하는 일도 없다. (물론, 당연히 원한다면 DI 하여 참조 가능하다.)
- 


## 어플리케이션 빈 (Application Bean)

- 개발자가 어떤 Bean 을 사용하겠다고 명시적으로 구성정보를 제공한 것을 의미한다.
- 그 제공된 구성정보(Configuration MetaData) 를 바탕으로 Spring Container 가 Bean으로 등록한 것들. 

- 어플리케이션 빈도 두가지로 구분이 가능하다.
	- 1. 어플리케이션 로직 빈 (Application Logic Bean)
		- 어플리케이션의 비지니스 로직, 도메인 로직을 담고 있는 Bean 을 의미함.
		- 즉, 개발자가 개발한다고 흔히 하는 코드들.
		- 즉, 사용자 구성정보를 이용해서 등록하는 Bean -> ComponentScan 을 이용해서 자동으로 Java 코드와 어노테이션으로 구성정보를 읽어온다. 
			- @ComponentScan 을 통해 Bean 이 등록되는 것!
		- ex> HelloController, SimpleHelloService
		
	- 2. 어플리케이션 인프라스트럭쳐 빈 (Application Infrastructure Bean)
		- 대부분 '기술'과 관련된 것.
		- 개발자가 직접 개발하지 않고, 이 어플리케이션에서는 이런 이런 기술을 사용하겠다 라고 명시적으로 Spring Container 에 구성정보를 제공하는 것.
		- 구성정보를 제공하지 않으면 Bean 으로 등록 되지 않는다.
		- 즉, 자동 구성 정보를 이용해서 등록하는 Bean -> AutoConfiguration 을 이용해서 구성정보를 읽어온다. 
			- @AutoConfiguration 을 통해 Bean 이 등록되는 것!
		- ex> DataSource, JpaEntityManagerFactory, JdbcTransactionManaber


## TomcatServletWebServerFactory 와 DispatcherServlet 은?

- 이 두가지는 어디에 속할까?
- 컨테이너 인프라스트럭처 빈이라고 생각 할 수 있지만 그렇지 않다. 
	- 명시적으로 선언을 해줘야지만 Bean 으로 등록이 되기 때문이다.
- 그렇기에, 이 두 클래스는 어플리케이션 인프라스트럭쳐 빈 으로 분리되는 것이 맞다.

- @Configuration 클래스 내부에서 @Bean 을 통해 등록되는 어플리케이션 인프라스트럭쳐 빈은 AutoConfiguration 이라는 자동 구성 정보 를 통해 Bean 으로 읽혀진다.




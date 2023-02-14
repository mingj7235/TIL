### Containerless Web Application Architecture

- Containerless 란? 
	- 컨테이너가 필요없다 라는 말이 아니다.
	- Serverless 의 의미와 비슷하다. 즉, Server 가 필요하지 않다가 아닌, Server 의 설정에 대해 신경을 개발자가 쓰지 않고 서버개발을 하는것과 비슷한 의미.
	- Container 관리를 신경 쓰지 않아도 된다는 말은 무엇일까?
	- Container 는 그럼 무엇일까? 
		- 스프링은 IoC 컨테이너다.
		- Web Component 는 Web Container 내부에 존재한다.
		- 즉, Web Component 를 관리하는 것이 Web Container 의 역할이다. 
			- Web Client 를 통해 요청이 들어오면, 어떤 Component 가 일을 처리할지를 Container 가 담당하여 지정해준다. 
		- Java의 언어로 변경하자면,
			- Web Container 는 Servlet Container, Web Component 는 Servlet 이 된다. 
			- Tomcat 은 잘 알려져 있는 Servlet Container 중 하나다. 
			- 전통적인 아주 기본적인 WEB 의 구조
			- Servlet Container 를 관리하고 생성하는 것이 전통적이었다.
		- 자, 그런데 여기서 Spring Container 는 무엇인가?
			- Servlet Container 뒤에 있는 것이 Spring Container 다.
			- Servlet 을 통해 들어온 요청을 Spring 이 받고, 그 내부에 있는 Bean 들이 요청에대한 동작을 하는 것. 
			- 근데 왜 Servlet Container 를 그냥 Spring Container 가 대체하면 안되는가? **안된다**. 기본적으로 Java 의 WEB 표준 기술을 사용하려면 servlet Container 가 존재해야하기 때문이다.
			- 그렇기에.. 어플리케이션을 동작시키기 위해서는 Servlet Container 를 무조건 필요로하고, 이것을 띄우는 것에 상당히 많은 공수가 들어간다. 
			- Spring 개발하려면 이걸 다 알아야할 필요가 있을까..?
			- 게다가 war 의 형식으로 빌드를 해야하고, 
		- photo



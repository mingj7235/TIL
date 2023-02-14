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
			- photo1
			- Servlet 을 통해 들어온 요청을 Spring 이 받고, 그 내부에 있는 Bean 들이 요청에대한 동작을 하는 것. 
			- 근데 왜 Servlet Container 를 그냥 Spring Container 가 대체하면 안되는가? **안된다**. 기본적으로 Java 의 WEB 표준 기술을 사용하려면 servlet Container 가 존재해야하기 때문이다.
			- 그렇기에.. 어플리케이션을 동작시키기 위해서는 Servlet Container 를 무조건 필요로하고, 이것을 띄우는 것에 상당히 많은 공수가 들어간다. 
			- 게다가 war 의 형식으로 빌드를 해야하고, 프로젝트 폴더 구성도 정해진대로 해야하며.. 신경 써야할 것들이 상당히 많다.
			- Spring 개발하려면 이걸 다 알아야할 필요가 있을까..?
			- 처음 설정을 하면 그 뒤로는 아예 보지도 않는 이러한 불필요한 설정들.. Servlet Container 를 띄우기 위한 번거로운 문제.
			- 특히나 Servlet Container 는 한가지가 아니다. Tomcat, Jetty 등등... 이런 Container 설정들은 아예 다르고, 버전마다도 상이하다. 이게 굉장히 진입장벽이 크다.
			- 그렇기에 Containerless Web Application Architecture 가 있으면 좋겠다 라는 요청이 나오게 된것 !
			- Servlet Container 가 필요없다! 라는 말이아니다. 이것을 개발자가 시간을 들여서 적용하는 수고를 제거해주면 좋겠다.. 라는 의미.
			- 실제로는 Servlet Container 가 존재하고 동작한다. 하지만, 이러한 설정을 개발자가 설정할 필요 없이 하도록 한 것이 Spring boot !
			- photo 2
		- Spring boot 에서는, Servlet Container 의 첫 시작을 시키는 것 까지도 제거해버리고, 클래스의 main 메소드를 호출하면 그것이 자동으로 실행되도록 Standalone application 의 방식으로 변경되었다. 엄청난일..!!
			- Servlet Container 에서 필요한 동작을 main 메소드가 실행되면서 모두 진행되도록 만듬.
			- 이것이 Containerless !




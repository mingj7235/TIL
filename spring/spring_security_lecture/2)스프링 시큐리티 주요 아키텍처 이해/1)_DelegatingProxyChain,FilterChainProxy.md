# 위임 필터 및 필터 빈 초기화 - DeligatingFilterProxy, FilterChainProxy

## 🍎 DelegatingFilterProxy

[] 사진

1. ``Servlet Filter`` 는 스프링에서 정의된 빈을 주입해서 사용할 수 없음

    - why? 👉🏻 ``Container`` 자체가 다르다!

    - ``Request`` 에 대한 접근 전, 그리고 응답 전에 ``Filter`` 가 작동함.

    - ``Servlet Container`` 에서 생성되고 실행된다.

    - 즉, 그렇기에 ``Spring Bean`` 을 주입받거나, ``Spring`` 에서 지원되는 기술을 사용하지 못한다.

        <i>- Spring Container 가 아닌, Servlet Container 에서 생성되기 때문이다.</i>

    - 💡 ``Filter`` 에서도 ``Spring`` 기술을 사용하고자 하는 요청이 생김. 

        - 이로 인해서 요청을 위임하기 위해 만들어진 것이 ``DelegatingFilterProxy`` 다.

        - ``Spring Security`` 는 ``Spring Bean`` 을 생성하고, ``Servlet Filter`` 를 구현한다.

    - ``DelegatingFilterProxy``

        - 🔑 ``Servlet Filter`` 다. <b>Spring 에서 관리하는 Filter 가 아니다.</b>

        - ``Servlet Filter`` 에서 요청을 받아서, 요청을 ``Spring`` 에서 관리하는 ``Filter`` 에게 위임하는 역할을 한다.

        - ``DelegatingFilterProxy`` 를 통해 ``Servlet Filter`` 가 `Spring Bean` 에서 보안처리가 가능하게 된 것이다. 

        - 즉, ``DelegatingFilterProxy`` 는 ``Servlet Filter`` 이기 때문에 가장 먼저 요청을 받게 되고, 그 요청을 ``Spring`` 에게 전달한다는 것. 이것이 핵심 ! 


2. ``DelegatingFilterProxy`` 는 특정한 이름을 가진 스프링 빈 (``springSecurityFilterChain``) 을 찾아 그 빈에게 요청을 위임 

    - ``DelegatingFilterProxy`` 는 ``springSecurityFilterChain`` 이름으로 생성된 빈 (`FilterChainProxy`)을 ``ApplicationContext`` 에서 찾아 요청을 위임. 단지 대리자! 

    - ``DelegatingFilterProxy`` 는 실제 보안 처리를 하지 않음


## 🍉 FilterChainProxy

1. ``springSecurityFilterChain`` 의 이름으로 생성되는 ``Spring Filter Bean`` 이며 ``Filter`` 타입이다. 

2. ``DelegatingFilterProxy ``로 부터 요청을 위임받고 실제로 보안을 처리 !

3. `Spring Security `초기화 시 생성되는 필터들을 관리하고 제어 ! 

    [] filter 목록 사진


    - ``Spring Security`` 가 기본적으로 생성하는 필터

    - 설정 클래스에서 API 추가 시 생성되는 필터

4. 사용자의 요청을 필터 숱서대로 호출하여 전달

5. 사용자 정의 필터를 생성해서 기존의 필터 전, 후로 추가 가능

    - 직접 ``Filter`` 를 구현하여 추가할 수 있음.

    - 필터의 순서를 잘 정의해야 한다. 

6. 마지막 필터까지 인증 및 인가 예외가 발생하지 않으면 보안을 통과.

    - 통과하면 최종적으로 ``Servlet`` 으로 보내어 자원을 사용하도록 함.


## 🤖 구조

[] 사진 

- ``Servlet Container`` 와 `Spring Container` 의 구분! 

- `Spring MVC / DispatcherServlet` : 최종 자원

    - `FilterChainProxy`를 통해 통과하면 최종 자원으로 request 전달.


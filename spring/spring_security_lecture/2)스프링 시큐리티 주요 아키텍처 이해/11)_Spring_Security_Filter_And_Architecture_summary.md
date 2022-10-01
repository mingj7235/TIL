# 스프링 시큐리티 필터 및 아키텍처 정리

## 💡 전체 흐름도 

[]

### 🍎 Spring Security 초기화 과정

- `SecurityConfig` (implements WebSecurityConfigurerAdapter)

    - `Configuration` 대로 `HttpSecurity` 클래스를 통해 `filter`들을 생성한다. 

- 각각의 `Filters` 들이 `WebSecurity` 에게 전달이 된다.

- `WebSecurity` 는 `FilterChainProxy` 에 `filters` 를 생성자로 전달한다. 

- `FilterChainProxy`

    - 각각의 `Filter` 들을 가지고 있다. 

- `DelegatingFilterProxy` 

    - `Servlet Filter` 인 `DelegatingFilterProxy` 은, `springSecurityFilterChain` 의 `Bean` 이름을 가진 `Bean` 클래스를 찾는데, 그 클래스가 `FilterChainProxy` 다.

    - 사용자의 요청을 받고 본인이 처리하지 않고, `Spring Container` 내부에 있는 `FilterChainProxy` 에게 요청을 위임한다.

<br>

### 🍉 사용자로부터의 요청 시작

<br>

`Filter` 들이 작동하기 시작함

<br>

- <b>DelegatingFilterProxy</b> 가 가장 먼저 요청을 받음

    - `FilterChainProxy` 에게 요청을 위임

- <b>FilterChainProxy</b>

    - `SpringSecurity` 초기화 때 생성된 각각의 ``Filter` 들에게 요청을 처리하도록 호출함 

    - 각 순서대로 `Filter` 들에게 요청을 수행하도록 한다.

    - `Filter` 들은 각각의 요청이 끝나면 다음의 `Filter` 들로 요청을 넘기게 된다.

- <b>SecurityContextPersistenceFilter  </b>

    - `SecurityContext` 를 조회하여 `Session` 에 저장된 이력이 있는지를 확인한다.

        - `loadContext` : `Session` 에 `SecurityContext` 객체가 있는지 확인하여 불러온다.

    - 처음 인증을 시도하거나 익명 사용자라면 `SecurityContext` 를 생성하여 `SecurityContextHolder` 안에 저장을 시킨다. 

    - `Session` 에 저장되어 있다면, `SecurityContext` 를 `Load` 시킨다.

        - 그리고 그 안에 있는 `Authentication` 객체를 꺼내어서 `UsernamePasswordAuthenticationFilter` 에게 전달한다. 

    - `Client` 에 응답하기 직전에 항상 `Clear SecurityContext` 를 통해 초기화 시킨다.

- <b>LogoutFilter</b>

    - `Logout` 요청이 없다면 이 필터는 역할이 없다. 

    - `Logout` 요청시에 `Logout` 을한다.

- <b>UsernamePasswordAuthenticationFilter</b>

    - `Form` 인증을 수행하는 필터 

    - `Authentication` 객체를 만들고, `AuthenticationManager` 를 통해 `AuthenticationProvider` 에게 인증을 위임하여 검증한다.

    - 인증이 완료되면 `SecurityContextHolder` 안의 `SecurityContext` 에 인증을 완료한 `Authentication` 객체를 저장한다. 

        - 여기서 `SecurityContext` 는 앞의 `SecurityContextPersistenceFilter` 를 통해 생성된 것이다. 

    - 인증에 성공 한 후에 `SessionManagementFilter` 를 통해 후처리를 한다.

        - 동시 접근 제어 세션 기능이 여기서 발동되는 것이다. (`ConcurrentSession`)

        - 세션 고정 보호 (`SessionFixation`) - `Session` 재 생성 및 쿠키 재생성

        - `Register` `SessionInfo` - `Session` 등록

        -> 인증 성공 후 3가지 작업이 `SessionManagermentFilter` 에서 이루어진다.

- <b>ConcurrentSessionFilter</b>

    - 동일한 계정이 두명 이상 접속시 작동하는 `Filter`

    - 만료된 사용자가 접속했을 때, `Logout` 을 시켜버리는 `Filter`

    => 최대 세션 허용 개수를 계속 유지시켜 준다. 

- <b>RememberMeAuthenticationFilter</b>

    - `Session` 이 만료되어 인증 객체가 `null` 인 경우에 `request header`를 확인하고, `remember-me cookie` 가 있을 때 인증처리를 시도한다.


- <b>AnonymousAuthenticationFilter</b>

    - 익명 사용자가 자원에 바로 접근을 시도하는 경우에 동작함

- <b>SessionManagementFilter</b>

    - `UsernamePasswordAuthenticationFilter` 가 작동할 때 함께 작동한다.

    - 단독으로 작동하는 경우 :  현재 세션에 `SecurityContext` 가 `null` 인경우, 혹은 `Session` 이 없는 경우에는 단독으로 작동한다. 

    - 전략

        1) `SessionAuthenticationException`

            - 현재 사용자 인증 시도 차단

        2) `session.expireNow`

            - 이전 사용자 세션 만료 설정 (default 값)

- <b>ExceptionTransLationFilter</b>

    - 인증 or 인가 예외가 발생했을 경우 예외만을 처리하는 필터다.

- <b>FilterSecurityInterceptor</b>

    - 인가 처리를 하는 필터

    - 1. `SecurityContext` 에 인증객체가 있는지 확인

    - 2. `SecurityContext` 에서 인증된 객체의 인가 정보를 확인하여 자원 접근 권한 확인








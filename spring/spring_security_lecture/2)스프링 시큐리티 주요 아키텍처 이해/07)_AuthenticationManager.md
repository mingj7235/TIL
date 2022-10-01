# AuthenticationManager

## 흐름도

[] 사진

인증 처리하는 `Filter` (`UsernamePasswordAuthenticationFilter`) 로 부터 인증 을 하라는 지시를 받는 클래스

`AuthenticationProvider` 목록 중에서 인증 처리 요건에 맞는 `AuthenticationProvider` 를 찾아 인증 처리를 위임한다.

직접 인증 처리를 하는것이 아니라, 적절한 `Provider` 에게 위임을 하는 역할이다. (말 그래도 매니징 !)

- 사용자 인증 요청

    - 인증의 방식이 ` Form, RememberMe, Oauth `등 여러가지 방법으로 인증을 한다.

- `ProviderManager` 가 전달받은 `Authentication` 정보를 `AuthenticationProvider` 들에게 전달을 하는데, 인증요처에 걸맞는 `AuthenticaitonProvider` 를 선택하여 인증을 위임한다.


- 부모의 `ProviderManager` 를 설정하여 `AuthenticaitonProvider` 를 계속 탐색 할 수 있다. 


- 즉, `Manager` 는 실제로 인증처리를 할 `Provider` 를 찾아서 위임을 하는 역할을 하는 것이다. 

> 인증에 성공하게 된다면, AuthenticaitonProvider 는 인증에 성공한 ProviderManager 에 전달하고, ProviderManager 는 본인을 호출한 인증 Filter 로 인증 성공 객체를 반환한다.


스프링 시큐리티가 초기화 되면서 부모 `ProviderManager` 를 생성하고, `default ProviderManager`를 생성한다. 







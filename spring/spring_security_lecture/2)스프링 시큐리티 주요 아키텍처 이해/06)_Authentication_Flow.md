# Authentication Flow

## 🤖 흐름도

인증의 흐름 ! 


### Form 인증 방식의 로그인 요청

[] 사진

<br>

- `UsernamePasswordAuthenticationFilter` 는 `Authentication` 객체에 요청으로 부터 전달 받은 ID 와 PW 를 담아 토큰 객체를 생성하여 `AuthenticationManager` 에 전달한다. 

- `AuthenticationManager` 는 `AuthenticationProvider` 에게 위임한다.

    - List 안에 있는 `AuthenticationProvider` 중 하나를 선택하여 인층 처리를 위임하는 역할

- `AuthenticationProvider`

    - 실질적으로 `ID, PW` 를 검증한다.

    - `ID 는 username` 을 의미한다. 

    - `UserDetailsService` 에 ID 를 전달하면서, 유저 객체를 요청한다. 

        - 여기에 `username` 을 전달한다. 즉, <b>ID 검증을 하는 것. </b>

- `UserDetailsService`

    - `DB`를 통해 유저 객체를 조회한다.

    - 유저가 있다면 `UserDetails` 타입으로 유저 객체를 반환한다. 

        - <b>UserDetials 타입으로 반환을 받았다는 것은 ID 는 검증이 되었다는 것임.</b>

        - 그 이후에 `PW` 를 검증한다. 

            - 반환 받은 `UserDetials 객체의 PW와 Authentication 객체에 저장된 입력받은 PW를 비교`한다.

            - PW 검증이 안되면 `Bad Credential` 예외를 발생 시킨다.

    - 유저가 없다면 없다면 `UsernamePasswordAuthenticationFilter` 를 통해 예외를 발생 시킨다. 


- 최종적으로 인증된 `Authentication` 을 `AuthenticationManager` 가 `UsernamePasswordAuthenticationFilter` 에 전달하고, 이것 (`Authentication` 인증객체) 을 `UsernamePasswordAuthenticationFilter` 가 `SecurityContext` 에 저장한다. 




    




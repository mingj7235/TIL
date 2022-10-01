# AuthenticationProvider

## 흐름도

[]

- 실질적인 인증 절차를 진행하는 클래스

- `AuthenticationProvider` 는 인터페이스다. 실무에서는 이것을 상황에 맞게 구현해서 사용한다.

- 자신을 위임한 `Manager` 로부터 `authentication` 객체를 전달 받음 (id, pw 가 저장되어 있는 객체)

- ID 검증

    - `UserDetailsService` 를 통해 검증하고, `UserDetails` 객체를 반환 받는다. 

    - `UserDetails` 객체를 전달 받게 되면 `ID` 가 검증이 된 것이다.

    - 인증 실패 시 `UserNotFoundException `

- PW 검증

    - 로그인시 전달 받은 `Authentication` 안에 있는 PW와, ID 검증을 통해 받아온 `UserDetails` 객체 안에 있는 PW 정보를 비교하여 검증을 한다.

    - 인증 실패 시 `BadCredentailException`

- 추가 검증까지 마친 후에는 `Authentication` 객체를 다시 생성하는데, 여기에는 `User` 객체와 권한 정보인 `authorities` 를 저장하고, `AuthenticationManager` 에게 전달 한다. 


>Filter -> Manager -> Provider -> UserDetailsService -> provider -> Manager -> Filter -> Security Context 에 저장 (전역적 사용을 위해) -> Session 에 저장


* 인증 예외는 호출한 Filter 로 전달 한다. 
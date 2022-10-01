# Authorization, FilterSecurityInterceptor


## 🍎 Authorizaiton

### '인가' 의 개념

- 무엇이 `허가` 되었는지 증명하는 것

- 즉, 특정 자원에 접근하기 위해 사용자가 허가 되었는가를 증명하는 것이다.

![스크린샷 2022-10-01 오후 5 13 47](https://user-images.githubusercontent.com/74750901/193401504-580baf7d-a3d6-4411-9fd5-bce6b6ac415f.png)
<i>출처 : 정수원님 강의 2-9) Authorization, FilterSecurityInterceptor </i>

- <strong>Authentication</strong>

    - 인증이 된 사용자 인지 아닌지를 확인하는 절차

<br>

- <strong>Authorization</strong>

    - 인증된 사용자가 특정 자원에 접근할 수 있는 자격이 있는지 확인하는 절차

<br>

### Spring Security 가 지원하는 권한 계층

![스크린샷 2022-10-01 오후 5 17 59](https://user-images.githubusercontent.com/74750901/193401510-a34e8327-75c2-4f11-9c54-fdb4dc77535b.png)
<i>출처 : 정수원님 강의 2-9) Authorization, FilterSecurityInterceptor </i>

- 웹 계층 - `/user`

    - URL 요청에 따른 메뉴 혹은 화면 단위의 레벨 보안

- 서비스 계층 - `user()`

    - 화면 단위가 아닌 메소드 같은 기능 단위의 레벨 보안


- 도메인 계층 -user (Access Control List, 접근 제어 목록) 

    - 객체 단위의 레벨 보안

<br>
 
## 🍉 FilterSecurityInterceptor

<br>

### 개념

- <b>마지막에 위치한 필터</b> 로써 `인증된 사용자에 대하여` 특정 요청의 승인 / 거부 여부를 최종적으로 결정. 즉, 자원의 접근 권한 여부를 체크 하는 것임.

- 인증객체 (`Authentication`) 없이 보호자원에 접근을 시도할 경우 `AuthenticationException` 을 발생

    - 인증객체가 없으면 인가 자체를 수행할 수 없음

- 인증 후 자원에 접근 가능한 권한이 존재하지 않을 경우 `AccessDeniedException` 을 발생

    - 인증은 받았으나, 자원에 접근하기 위한 권한이 없는 경우임

> 두가지를 모두 체크한다. 즉, 1) 인증객체가 있는지, 그리고 2) 그 인증객체가 권한이 있는지

- 권한 제어 방식 중 ``HTTP 자원의 보안을 처리``하는 필터

- 권한 처리를 `AccessDecisionManager` 에게 맡김

* 인가 라는 것은 결국 `자원에 접근` 이라는 개념이므로 `Access` 라는 단어가 사용된다.


### 흐름도

![스크린샷 2022-10-01 오후 5 25 35](https://user-images.githubusercontent.com/74750901/193401519-4417b5db-80b7-4a28-8eba-dc8e52dbc135.png)
<i>출처 : 정수원님 강의 2-9) Authorization, FilterSecurityInterceptor </i>

- 인증객체는 `SecurityContext` 안에 저장되어 있다.

    - 인증을 받았다면 `SecurityContext` 안에 저장되어 있을 것이다. 

    - `FilterSecurityInterceptor` 는 `SecurityContext` 에서 `Authentication` 객체를 확인한다. 

- `SecurityMetadataSource`

    - 사용자가 요청한 자원에 필요한 권한 정보를 조회한다.

    - 즉, `Authentication` 객체에서 `Authorities` 를 확인한다. 

    - 이 권한 정보가 `null` 이 아닌경우, `AccessDecisionManager` 에게 전달한다.

`AccessDecisionManager`

    - 최종 심의 결정자.

    - 즉, 전달받은 권한 정보를 심의하는데, `AccessDecisionVoter` 에게 전달하여 심의 요청을 한다.

`AccessDecisionVoter`

    - 심의를 하여서 `Manager` 에게 승인 / 거부 여부를 전달한다. 



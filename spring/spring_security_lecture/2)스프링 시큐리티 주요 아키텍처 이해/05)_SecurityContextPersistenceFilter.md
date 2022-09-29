# SecurityContextPersistenceFilter

## 🌱 개념

<b>SecurityContext 객체의 생성, 저장, 조회</b> 역할 하는 Filter 

- 익명 사용자 접근 시

    - 새로운 ``SecurityContext`` 객체를 생성하여 `SecurityContextHolder` 에 저장

    - `AnonymousAuthenticationFilter` 에서 `AnonymousAuthenticationToken` 객체를 `SecurityContext` 에 `Authentication`(익명 사용자 객체) 을 저장

- 인증 시 (여전히 인증 되기 전임)

    - 새로운 `SecurityContext` 객체를 생성하여 `SecurityContextHolder` 에 저장

    - `UsernamePasswordAuthenticationFilter` 에서 인증 성공 후 `SecurityContext` 에 `UsernamePasswordAuthenticationToken` 객체를 `SecurityContext` 에 `Authentication` 을 저장

    - 인증이 최종 완료되면 ``Session`` 에 `SecurityContext` 를 저장

- 인증 후

    - 새로운 `SecurityContext` 를 생성하지 않는다.

    - ``Session`` 에서 `SecurityContext` 꺼내어 `SecurityContextHolder` 에 저장

    - `SecurityContext` 안에 `Authentcation` 객체가 존재하면 계속 인증을 유지한다.

- 최종 응답 시 공통

    - ``SecurityContextHolder.clearContext()``

        - 응답하면 항상 ``SecurityContextHolder`` 에 있는 ``SecurityContext`` 를 삭제한다.

        - 왜? 매 요청마다 <b> 익명사용자든, 인증 시점이든, 인증 후이든 ``SecurityContext`` 를 `SecurityContextHolder`에 저장하기 때문이다. </b>


- ``SecurityContextPersistenceFilter`` 는 결국 ``SecurityContext`` 를 ``SecurityContextHolder`` 에 저장시키는 역할을 하는 것이다.

    - 익명사용자 접근 시, 인증 시는 새로운 ``SecurityContext`` 를 생성하여 ``SecurityContextHolder`` 에 저장

    - 인증 후에는 ``Session`` 에서 `SecurityContext` 를 꺼내어 `SecurityContextHolder` 에 저장

- `Filter` 중에서 두번째로 존재하는 필터다.

## 🤖 흐름도 

[] 사진
<i>출처 : 정수원님 강의 2-5) 인증 저장소 필터 - SecurityContextPersistenceFilter </i>

- 매 ``Request`` 마다 ``SecurityContextPersistenceFilter`` 를 거친다.

- 인증 전 YES : 익명사용자 접근 or 인증 시 두가지 경우를 의미한다. 

- 인증 전 NO : 인증이 성공한 후의 접근을 의미한다. 

[] 사진
<i>출처 : 정수원님 강의 2-5) 인증 저장소 필터 - SecurityContextPersistenceFilter </i>

- 인증을 받기 전의 ``SecurityContext`` 는 ``null`` 이다. ``Authentication`` 객체가 없다. 
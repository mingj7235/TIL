# SessionManagementFilter, ConcurrentSessionFilter


## 🍎 SessionManagementFilter

- 세션관리

    - 인증 시 사용자의 세션정보를 등록, 조회, 삭제 등의 세션 이력을 관리

- 동시적 세션 제어 (ConcurrentSessionFilter 와 맞물려서 제어함)

    - 동일 계정으로 접속이 허용되는 최대 세션수를 제한

- 세션 고정 보호

    - 인증할 때마다 세션쿠키를 새로 발급하여 공격자의 쿠키 조작을 방지

- 세션 생성 정책

    - Always, If_Required, Never, Stateless

<br>

## 🍉 ConcurrentSessionFilter

- 매 요청마다 현재 사용자의 세션 만료 여부를 체크 -> 동시적 세션 처리 

- 세션이 만료되었을 경우 즉시 만료 처리를 함. 

```java
    session.isExpired() == true // 이 경우가 세션이 만료되었다는 의미

    // 만료되었을 때
    // - 즉시 로그아웃 처리
    // - 로그아웃 처리 후 즉시 오류 페이지 응답 "This session has been expired"
```

## 🔑 동작 원리

[]

``SessionManagementFilter``, ``ConcurrentSessionFilter`` 가 맞물려서 동시적 세션제어를 함


[]

SessionManagementFilter

    - ConcurrentSessionControlAuthenticationStrategy

    - ChangeSessionIdAuthenticationStrategy

    - RegisterSessionAuthenticationStrategy

## 상황

``🙋🏻‍♂️ User 1`` 이 로그인 시도

1. ``ConcorrentSessionControlAuthenticationStrategy``
    
    - ``session`` 을 count 한다. 최대 허용 개수를 비교하여 동시 접속 session 을 제어한다.
     

2. ``ChangeSessionIdAuthenticationStrategy``

    - 세션 고정보호 하는 클래스 (session.changeSessionId())

3. ``RegisterSessionAuthenticationStrategy``

    - 사용자의 세션 정보를 등록하는 클래스

    - 이 클래스를 통해 Session 을 등록하기 때문에, 이때 session count 가 1이된다.

<br>

``🙋🏻 User 2``가 ``🙋🏻‍♂️ User 1``과 동일한 계정으로 로그인 시도 

1. ``sessionCount == maxSessions`` 일 경우 
    - 인증 실패 전략인 경우 
        -> User2 에게 인증 실패 예외 (``SessionAuthenticationException``) 

    - 세션 만료 전략인경우
        -> User1의 세션을 만료시킨다. ``session.expireNow()``
        -> User2 는 인증에 성공한다. 

        -> 이 경우, ``🙋🏻‍♂️ User 1`` 이 다시 로그인할 때, ``ConcurrentSessionFilter``가 사용자의 세션만료를 계속 체크하고, 이 Filter를 통해 ``session.isExpired()``가 감지되어 Logout 이 되고, 세션 만료 응답을 받는다. 

        * ``ConcurrentSessionFilter`` 는 계속 세션 만료를 요청마다 체크한다. 


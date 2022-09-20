# RememberMeAuthenticationFilter

## 동작 조건

<br>

### 1. ``Authentication`` 객체가 ``null`` 인 경우

- 인증을 받은 사용자의 경우 ``Security Context`` 안에 ``Authentication`` 이 존재한다.

💡 ``Authentication`` 객체가 ``null`` 인 경우 

인증객체는 ``Security Context`` 에 저장되어 있다.

사용자 세션이 만료되거나 세션이 끊겨서 더이상 세션에서 ``Security Context``를 찾지 못함
``Security Context`` 가 존재하지 않기에 ``Authentication`` 객체가 ``null`` 이 됨.

- 🔑 ``Authentication`` 객체가 ``null`` 이 ``아니면`` 작동하지 않는다.

>Session 이 더 이상 활성화되지 않아서 SC 에서 Authentication 객체를 찾을 수 없는 경우에 RememberMeAuthenticationFilter 가 작동하는 것 !!

### 2. 사용자가 Form 인증을 받을 때, RememberMe 쿠키 값을 헤더에 가진 채 요청을 하는 경우

<br>

이 2가지 조건이 충족될 때, ``RememberMeAuthenticationFilter`` 가 작동한다.

<br>

(Session 만료, RememberMe 쿠키를 가지고 요청)

![스크린샷 2022-09-20 오후 9 52 29](https://user-images.githubusercontent.com/74750901/191268023-2bf283cc-e043-4787-8e36-4259b6cceed1.png)
<i>출처 : 정수원님 강의 1-7 Remember Me 인증 필터</i>


RememberMeServices

    TokenBasedRememberMeServices : Memory 에 저장된 Token 과 비교하는 구현체 (14일 default)

    PersistentTokenBasedRememberMeSErvices : DB 에 저장된 Token 과 비교하는 구현체




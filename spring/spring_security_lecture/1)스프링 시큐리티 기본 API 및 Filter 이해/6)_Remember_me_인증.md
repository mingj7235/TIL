# Remember Me 인증

- 세션이 만료되고 웹 브라우저가 종료된 후에도 어플리케이션이 사용자를 기억하는 기능

- Rmember-Me 쿠키에 대한 Http 요청을 확인한 후 토큰 기반 인증을 사용해 유효성을 검사하고, 토큰이 검증되면 사용자는 로그인이 된다.

- 사용자 라이프 사이클

    - 인증 성공 (Remember-Me 쿠키 설정)
    - 인증 실패 (Remember-Me 쿠키가 존재하면 쿠키 무효화)
    - 로그아웃 (Remember-Me 쿠키가 존재하면 쿠키 무효화)


## Remember Me 인증 API

```java
    protected void configure(HttpSecurity http) throws Exception {
        http.rememberMe()
                .rememberMeParameter("remember") // 파라미터 명. default 값은 remember-me
                .tokenValiditySeconds(3600) // dafault 는 14일 
                .alwasRemember (true) // Remeberme 기능이 활성화 되지않아도 항상 실행 (일반적으로 false 를 한다.)
                .userDetailsService(userDetailsService) // 꼭 필요한 설정
                ;
    }
```

``인증이 되었다.`` 라는 의미 

    Spring Security 에서 Session 을 만들었고,
    그 Session 에 인증객체를 저장함. 



``RemeberMeAuthenticationFilter`` 가 remember-me 라는 쿠키를 생성 해서 발급해준다.
    remember-me 쿠키가 있을 경우, 그 쿠키를 체크해서 Session 을 유지해준다. 
    그렇기 때문에 JSESSIONID 가 지워져도 다시 생성해준다. 







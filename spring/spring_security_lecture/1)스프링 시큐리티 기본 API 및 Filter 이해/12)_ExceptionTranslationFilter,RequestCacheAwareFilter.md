# ExceptionTranslationFilter, RequestCacheAwareFilter

## 인증 / 인가 API - ExceptionTranslationFilter

<br>

### 👉🏻 FilterSecurityInterceptor

- ``Spring Security`` 가 관리하는 `Filter` 중 가장 마지막에 있음

- 이 필터 직전의 필터가 이 챕터 주제중 하나인 ``ExceptionTranslationFilter`` 다.

- ``FilterSecurityInterceptor`` 에서 생긴 예외는 `ExceptionTranslationFilter` 로 예외를 throw를 하고 있다.



<br>

### 💡 ExceptionTranslationFilter

🍎 AuthenticationException

- ``인증`` 예외 처리

    1. AuthenticationEntryPoint 호출

        - 인증 예외가 발생 시, 다시금 인증을 할 수 있게끔 하는 인터페이스
        
        - 로그인 페이지 이동 or 401 오류 코드 전달 등

        - 개발자가 ``implements`` 받아 구현할 수 있다.


    2. 인증 예외가 발생하기 전의 요청 정보를 저장
        - ``RequestCache`` - 사용자의 ``이전 요청 정보``를 ``세션``에 저장하고 이를 꺼내오는 캐시 메커니즘
            - ``savedRequest`` - 사용자가 요청했던 request 파라미터 값들, 그 당시 헤더값들 등이 저장됨

        - 이전에 가고자 했던 자원 정보를 캐싱하기 때문에, 인증이 된다면 바로 그 자원으로 접근 할 수 있도록 해주는 클래스가 ``RequestCache`` 다. 

        - ``요청정보가 저장된`` 구현체는 ``SavedRequest``고, 이 ``저장된 정보를 세션``에 저장하는 클래스가 `RequestCache` 다. 



🍉  AccessDeniedException

- ``인가`` 예외 처리

    - 권한 예외에 대한 처리를 의미한다.

    - `AccessDeniedHandler` 에서 예외 처리하도록 제공


## 🔧 작동 원리


[] 사진

상황 : 인증 없이 바로 ``/user`` 자원으로 접근하는 상황. 즉, 익명 사용자가 접근.

- 사실상 인증 예외가 아닌 인가 예외 (왜? ``ANONYMOUS``라는 ``Role`` 이 있으므로)

- 하지만, `ANONYMOUS` 이므로, ``AuthenticationException`` (인증예외) 로 처리한다.

- 사실상 인증예외가 아닌 인가 예외이지만, `ANONYMOUS` 사용자는 인증 예외로 ``AuthenticationException`` 을 발생시킨다. 


* ``FilterSecurityInterceptor`` 가 예외를 던지는 것 !


## 🤖 API

### API 설명

```java

    protected void configure (HttpSecurity http) throws Exception {
        http.exceptionHandling()
                .authenticatonEntryPoint(authenticationEntryPoint()) // 인증 실패시 처리
                .accessDeniedHanler(accessDeniedHandler()) // 인가 실패시 처리
    }

```

### API 구현

```java

    // 인증에 실패했다가 다시 성공했을 때, 이전 세션에 저장되었던 이전의 요청정보로 이동하기위해 설정

    http
        .formLogin()
        .successHandler(new AuthenticationSuccessHandler() {
            @Override
            public void onAuthenticationSuccess(final HttpServletRequest request, final HttpServletResponse response, final Authentication authentication) throws IOException, ServletException {
                RequestCache requestCache = new HttpSessionRequestCache(); // filter에서 사용자가 원래 가고자 했던 요청 정보를 꺼내기 위해서
                SavedRequest savedRequest = requestCache.getRequest(request, response); //사용자가 원래 가고자했던 request 정보
                String redirectUrl = savedRequest.getRedirectUrl();
                response.sendRedirect(redirectUrl); //인증에 성공하면 세션에 저장되어있던 이전의 요청정보로 이동하도록 설정한 것
            }
        });

    http
        .exceptionHandling()
        .authenticationEntryPoint(new AuthenticationEntryPoint() {
            @Override
            public void commence(final HttpServletRequest request, final HttpServletResponse response, final AuthenticationException authException) throws IOException, ServletException {
                response.sendRedirect("/login");
            }
        }) // 인증 실패 시 처리
        .accessDeniedHandler(new AccessDeniedHandler() {
            @Override
            public void handle(final HttpServletRequest request, final HttpServletResponse response, final AccessDeniedException accessDeniedException) throws IOException, ServletException {
                response.sendRedirect("/denied");
            }
        }) // 인가 실패 시 처리
        ;
```

<br>

### Lambda 로 동일한 로직 구현

```java

    // lambda 로 구현

    http
        .formLogin()
        .successHandler((request, response, authentication) -> {
            RequestCache requestCache = new HttpSessionRequestCache(); // filter에서 사용자가 원래 가고자 했던 요청 정보를 꺼내기 위해서
            SavedRequest savedRequest = requestCache.getRequest(request, response); //사용자가 원래 가고자했던 request 정보
            String redirectUrl = savedRequest.getRedirectUrl();
            response.sendRedirect(redirectUrl); //인증에 성공하면 세션에 저장되어있던 이전의 요청정보로 이동하도록 설정한 것
        });


    http
        .exceptionHandling()
        .authenticationEntryPoint(
                (request, response, authException) -> response.sendRedirect("/login")) 
        .accessDeniedHandler(
                (request, response, accessDeniedException) -> response.sendRedirect("/denied")) 
        ;    
```


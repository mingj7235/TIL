# ExceptionTranslationFilter, RequestCacheAwareFilter

## ์ธ์ฆ / ์ธ๊ฐ API - ExceptionTranslationFilter

<br>

### ๐๐ป FilterSecurityInterceptor

- ``Spring Security`` ๊ฐ ๊ด๋ฆฌํ๋ `Filter` ์ค ๊ฐ์ฅ ๋ง์ง๋ง์ ์์

- ์ด ํํฐ ์ง์ ์ ํํฐ๊ฐ ์ด ์ฑํฐ ์ฃผ์ ์ค ํ๋์ธ ``ExceptionTranslationFilter`` ๋ค.

- ``FilterSecurityInterceptor`` ์์ ์๊ธด ์์ธ๋ `ExceptionTranslationFilter` ๋ก ์์ธ๋ฅผ throw๋ฅผ ํ๊ณ  ์๋ค.



<br>

### ๐ก ExceptionTranslationFilter

๐ AuthenticationException

- ``์ธ์ฆ`` ์์ธ ์ฒ๋ฆฌ

    1. AuthenticationEntryPoint ํธ์ถ

        - ์ธ์ฆ ์์ธ๊ฐ ๋ฐ์ ์, ๋ค์๊ธ ์ธ์ฆ์ ํ  ์ ์๊ฒ๋ ํ๋ ์ธํฐํ์ด์ค
        
        - ๋ก๊ทธ์ธ ํ์ด์ง ์ด๋ or 401 ์ค๋ฅ ์ฝ๋ ์ ๋ฌ ๋ฑ

        - ๊ฐ๋ฐ์๊ฐ ``implements`` ๋ฐ์ ๊ตฌํํ  ์ ์๋ค.


    2. ์ธ์ฆ ์์ธ๊ฐ ๋ฐ์ํ๊ธฐ ์ ์ ์์ฒญ ์ ๋ณด๋ฅผ ์ ์ฅ
        - ``RequestCache`` - ์ฌ์ฉ์์ ``์ด์  ์์ฒญ ์ ๋ณด``๋ฅผ ``์ธ์``์ ์ ์ฅํ๊ณ  ์ด๋ฅผ ๊บผ๋ด์ค๋ ์บ์ ๋ฉ์ปค๋์ฆ
            - ``savedRequest`` - ์ฌ์ฉ์๊ฐ ์์ฒญํ๋ request ํ๋ผ๋ฏธํฐ ๊ฐ๋ค, ๊ทธ ๋น์ ํค๋๊ฐ๋ค ๋ฑ์ด ์ ์ฅ๋จ

        - ์ด์ ์ ๊ฐ๊ณ ์ ํ๋ ์์ ์ ๋ณด๋ฅผ ์บ์ฑํ๊ธฐ ๋๋ฌธ์, ์ธ์ฆ์ด ๋๋ค๋ฉด ๋ฐ๋ก ๊ทธ ์์์ผ๋ก ์ ๊ทผ ํ  ์ ์๋๋ก ํด์ฃผ๋ ํด๋์ค๊ฐ ``RequestCache`` ๋ค. 

        - ``์์ฒญ์ ๋ณด๊ฐ ์ ์ฅ๋`` ๊ตฌํ์ฒด๋ ``SavedRequest``๊ณ , ์ด ``์ ์ฅ๋ ์ ๋ณด๋ฅผ ์ธ์``์ ์ ์ฅํ๋ ํด๋์ค๊ฐ `RequestCache` ๋ค. 



๐  AccessDeniedException

- ``์ธ๊ฐ`` ์์ธ ์ฒ๋ฆฌ

    - ๊ถํ ์์ธ์ ๋ํ ์ฒ๋ฆฌ๋ฅผ ์๋ฏธํ๋ค.

    - `AccessDeniedHandler` ์์ ์์ธ ์ฒ๋ฆฌํ๋๋ก ์ ๊ณต


## ๐ง ์๋ ์๋ฆฌ

![แแณแแณแแตแซแแฃแบ 2022-09-25 แแฉแแฎ 11 11 06](https://user-images.githubusercontent.com/74750901/192149676-e6d1c304-e3fb-41bd-8a23-fe10df1873c5.png)
<i>์ถ์ฒ : ์ ์์๋ ๊ฐ์ 1-12) ์์ธ ์ฒ๋ฆฌ ๋ฐ ์์ฒญ ์บ์ ํํฐ : ExceptionTranslationFilter, RequestCacheAwareFilter </i>


์ํฉ : ์ธ์ฆ ์์ด ๋ฐ๋ก ``/user`` ์์์ผ๋ก ์ ๊ทผํ๋ ์ํฉ. ์ฆ, ์ต๋ช ์ฌ์ฉ์๊ฐ ์ ๊ทผ.

- ์ฌ์ค์ ์ธ์ฆ ์์ธ๊ฐ ์๋ ์ธ๊ฐ ์์ธ (์? ``ANONYMOUS``๋ผ๋ ``Role`` ์ด ์์ผ๋ฏ๋ก)

- ํ์ง๋ง, `ANONYMOUS` ์ด๋ฏ๋ก, ``AuthenticationException`` (์ธ์ฆ์์ธ) ๋ก ์ฒ๋ฆฌํ๋ค.

- ์ฌ์ค์ ์ธ์ฆ์์ธ๊ฐ ์๋ ์ธ๊ฐ ์์ธ์ด์ง๋ง, `ANONYMOUS` ์ฌ์ฉ์๋ ์ธ์ฆ ์์ธ๋ก ``AuthenticationException`` ์ ๋ฐ์์ํจ๋ค. 


* ``FilterSecurityInterceptor`` ๊ฐ ์์ธ๋ฅผ ๋์ง๋ ๊ฒ !


## ๐ค API

### API ์ค๋ช

```java

    protected void configure (HttpSecurity http) throws Exception {
        http.exceptionHandling()
                .authenticatonEntryPoint(authenticationEntryPoint()) // ์ธ์ฆ ์คํจ์ ์ฒ๋ฆฌ
                .accessDeniedHanler(accessDeniedHandler()) // ์ธ๊ฐ ์คํจ์ ์ฒ๋ฆฌ
    }

```

### API ๊ตฌํ

```java

    // ์ธ์ฆ์ ์คํจํ๋ค๊ฐ ๋ค์ ์ฑ๊ณตํ์ ๋, ์ด์  ์ธ์์ ์ ์ฅ๋์๋ ์ด์ ์ ์์ฒญ์ ๋ณด๋ก ์ด๋ํ๊ธฐ์ํด ์ค์ 

    http
        .formLogin()
        .successHandler(new AuthenticationSuccessHandler() {
            @Override
            public void onAuthenticationSuccess(final HttpServletRequest request, final HttpServletResponse response, final Authentication authentication) throws IOException, ServletException {
                RequestCache requestCache = new HttpSessionRequestCache(); // filter์์ ์ฌ์ฉ์๊ฐ ์๋ ๊ฐ๊ณ ์ ํ๋ ์์ฒญ ์ ๋ณด๋ฅผ ๊บผ๋ด๊ธฐ ์ํด์
                SavedRequest savedRequest = requestCache.getRequest(request, response); //์ฌ์ฉ์๊ฐ ์๋ ๊ฐ๊ณ ์ํ๋ request ์ ๋ณด
                String redirectUrl = savedRequest.getRedirectUrl();
                response.sendRedirect(redirectUrl); //์ธ์ฆ์ ์ฑ๊ณตํ๋ฉด ์ธ์์ ์ ์ฅ๋์ด์๋ ์ด์ ์ ์์ฒญ์ ๋ณด๋ก ์ด๋ํ๋๋ก ์ค์ ํ ๊ฒ
            }
        });

    http
        .exceptionHandling()
        .authenticationEntryPoint(new AuthenticationEntryPoint() {
            @Override
            public void commence(final HttpServletRequest request, final HttpServletResponse response, final AuthenticationException authException) throws IOException, ServletException {
                response.sendRedirect("/login");
            }
        }) // ์ธ์ฆ ์คํจ ์ ์ฒ๋ฆฌ
        .accessDeniedHandler(new AccessDeniedHandler() {
            @Override
            public void handle(final HttpServletRequest request, final HttpServletResponse response, final AccessDeniedException accessDeniedException) throws IOException, ServletException {
                response.sendRedirect("/denied");
            }
        }) // ์ธ๊ฐ ์คํจ ์ ์ฒ๋ฆฌ
        ;
```

<br>

### Lambda ๋ก ๋์ผํ ๋ก์ง ๊ตฌํ

```java

    // lambda ๋ก ๊ตฌํ

    http
        .formLogin()
        .successHandler((request, response, authentication) -> {
            RequestCache requestCache = new HttpSessionRequestCache(); // filter์์ ์ฌ์ฉ์๊ฐ ์๋ ๊ฐ๊ณ ์ ํ๋ ์์ฒญ ์ ๋ณด๋ฅผ ๊บผ๋ด๊ธฐ ์ํด์
            SavedRequest savedRequest = requestCache.getRequest(request, response); //์ฌ์ฉ์๊ฐ ์๋ ๊ฐ๊ณ ์ํ๋ request ์ ๋ณด
            String redirectUrl = savedRequest.getRedirectUrl();
            response.sendRedirect(redirectUrl); //์ธ์ฆ์ ์ฑ๊ณตํ๋ฉด ์ธ์์ ์ ์ฅ๋์ด์๋ ์ด์ ์ ์์ฒญ์ ๋ณด๋ก ์ด๋ํ๋๋ก ์ค์ ํ ๊ฒ
        });


    http
        .exceptionHandling()
        .authenticationEntryPoint(
                (request, response, authException) -> response.sendRedirect("/login")) 
        .accessDeniedHandler(
                (request, response, accessDeniedException) -> response.sendRedirect("/denied")) 
        ;    
```


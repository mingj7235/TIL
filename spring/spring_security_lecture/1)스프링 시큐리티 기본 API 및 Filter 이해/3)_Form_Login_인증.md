# Form Login 인증

### http.formLogin() : Form 로그인 인증 기능이 작동함

<br>

```java
    protected void configure (HttpSecurity http) throws Exception {
        http.formLogin()
                .loginPage("/loginPage") // login 하도록 하는 page로 이동 하는 url
                .defaultSuccessUrl("/") // login 성공 후 이동 페이지
                .failureUrl("/login.html?error=true") // login 실패 후 이동 페이지 
                .usernameParameter("userId") // id 파라미터 명 설정 (form tag 네임)
                .passwordParameter("passwd") // pw 파라미터 명 설정 (form tag 네임)
                .loginProcessingUrl("/loginProc") //form tag의 action url이 여기에 매핑되는 것임. login 처리 api url 설정
                .successHandler(loginSuccessHandler()) //login 성공 후 핸들러
                .failureHandler(loginFailureHandler()) // login 실패 후 핸들러
    }
```

### 🔧 실제 코드

<br>

```java

    protected void configure (HttpSecurity http) throws Exception {
        http.formLogin()
                .loginPage("/loginPage") 
                .defaultSuccessUrl("/")
                .failureUrl("/login") 
                .usernameParameter("userId") 
                .passwordParameter("passwd") 
                .loginProcessingUrl("/login") 
                .successHandler(new AuthenticationSuccessHandler() {
                    @Override
                    public void onAuthenticationSuccess(final HttpServletRequest request, final HttpServletResponse response, final Authentication authentication) throws IOException, ServletException {
                        System.out.println("authentication : " + authentication.getName());
                        response.sendRedirect("/");
                    }
                })
                .failureHandler(new AuthenticationFailureHandler() {
                    @Override
                    public void onAuthenticationFailure(final HttpServletRequest request, final HttpServletResponse response, final AuthenticationException exception) throws IOException, ServletException {
                        System.out.println("exception : " + exception.getMessage());
                        response.sendRedirect("/login");
                    }
                })
                .permitAll(); //login 페이지는 아무 인증없이도 들어와야 하므로 !
```
# AuthenticationSuccessHandler, AuthenticationFailureHandler

## 🍎 SuccessHandler


```java

@Component
public class CustomAuthenticationSuccessHandler extends SimpleUrlAuthenticationSuccessHandler {

    private RequestCache requestCache = new HttpSessionRequestCache();

    private RedirectStrategy redirectStrategy = new DefaultRedirectStrategy();

    @Override
    public void onAuthenticationSuccess(final HttpServletRequest request, final HttpServletResponse response, final Authentication authentication) throws IOException, ServletException {

        setDefaultTargetUrl("/"); 

        SavedRequest savedRequest = requestCache.getRequest(request, response); 

        if (savedRequest != null) { 
            String targetUrl = savedRequest.getRedirectUrl();
            redirectStrategy.sendRedirect(request, response, targetUrl);
        } else {
            redirectStrategy.sendRedirect(request, response, getDefaultTargetUrl());
        }
    }

}
```

- `RequestCache`

    - 인증을 시도 할 때 접근했던 자원들의 정보를 가져오기위해

- `RedirectStrategy`

    - 말 그대로 리다이렉트 전략을 위해서 존재

- `setDefaultTargetUrl()`

    - 상속받은 `SimpleUrlAuthenticationSuccessHandler` 에 존재하는 메소드

    - 인증 성공 시 기본 타겟 `URL` 정보를 설정한다.

- `redirectStrategy.sendRedirect()`

    - 인증 성공시 저장된 `cache` 여부에 따라 페이지를 이동시킨다.

<br>


## 🍊 FailureHandler


### CustomAuthenticationFailureHandler.java

<br>

```java
@Component
public class CustomAuthenticationFailureHandler extends SimpleUrlAuthenticationFailureHandler {

    @Override
    public void onAuthenticationFailure(final HttpServletRequest request, final HttpServletResponse response, final AuthenticationException exception) throws IOException, ServletException {

        String errorMessage = "Invalid Username or Password";

        if (exception instanceof BadCredentialsException) {
            errorMessage = "Invalid Username or Password";
        } else if (exception instanceof InsufficientAuthenticationException) {
            errorMessage = "Invalid Secret key";
        }

        setDefaultFailureUrl("/login?error=true&exception=" + errorMessage);

        super.onAuthenticationFailure(request, response, exception);

    }

}

```

- 인증 실패 시 핸들러 설정이다.

- `setDefaultFailureUrl()` 메소드를 통해 인증 실패 시 보낼 컨트롤러를 지정한다. (GET)

<br>

### LoginController.java

```java
  @GetMapping ("/login")
    public String login(@RequestParam (value = "error", required = false) String error,
                        @RequestParam (value = "exception", required = false) String exception,
                        Model model) {

        model.addAttribute("error", error);
        model.addAttribute("exception", exception);
        return "user/login/login";
    }
```

- `login` 컨트롤러에서 `error` 와 `exception` 은 필수 파라미터가 아니기때문에 `required` 는 `false` 로 설정한다.

- `model` 에 담아 `view` 로 전달해준다.

<br>

### SecurityConfig.java

```java
...
  @Override
    protected void configure(final HttpSecurity http) throws Exception {
        http
                .authorizeRequests()
                    .antMatchers("/", "/users", "user/login/**", "/login*").permitAll()

...

```

- `/login*` : 뒤에 파라미터가 붙는 경로를 `permitAll()` 로 설정해주어야 한다.

<br>

### login.html

```html
...
<div th:if="${param.error}" class="form-group">
    <span th:text="${exception}" class="alert alert-danger"></span>
</div>
...
```

- param.error 값이 `true` 로 넘어오는데, 타임리프의 `th:if` 절에 `true` 나 `false` 로 문자열이 넘어오면 자동으로 `boolean` 으로 인식한다. 
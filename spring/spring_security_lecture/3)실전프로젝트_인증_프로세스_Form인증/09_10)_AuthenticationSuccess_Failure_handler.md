# AuthenticationSuccessHandler, AuthenticationFailureHandler

## π SuccessHandler


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

    - μΈμ¦μ μλ ν  λ μ κ·Όνλ μμλ€μ μ λ³΄λ₯Ό κ°μ Έμ€κΈ°μν΄

- `RedirectStrategy`

    - λ§ κ·Έλλ‘ λ¦¬λ€μ΄λ νΈ μ λ΅μ μν΄μ μ‘΄μ¬

- `setDefaultTargetUrl()`

    - μμλ°μ `SimpleUrlAuthenticationSuccessHandler` μ μ‘΄μ¬νλ λ©μλ

    - μΈμ¦ μ±κ³΅ μ κΈ°λ³Έ νκ² `URL` μ λ³΄λ₯Ό μ€μ νλ€.

- `redirectStrategy.sendRedirect()`

    - μΈμ¦ μ±κ³΅μ μ μ₯λ `cache` μ¬λΆμ λ°λΌ νμ΄μ§λ₯Ό μ΄λμν¨λ€.

<br>


## π FailureHandler


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

- μΈμ¦ μ€ν¨ μ νΈλ€λ¬ μ€μ μ΄λ€.

- `setDefaultFailureUrl()` λ©μλλ₯Ό ν΅ν΄ μΈμ¦ μ€ν¨ μ λ³΄λΌ μ»¨νΈλ‘€λ¬λ₯Ό μ§μ νλ€. (GET)

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

- `login` μ»¨νΈλ‘€λ¬μμ `error` μ `exception` μ νμ νλΌλ―Έν°κ° μλκΈ°λλ¬Έμ `required` λ `false` λ‘ μ€μ νλ€.

- `model` μ λ΄μ `view` λ‘ μ λ¬ν΄μ€λ€.

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

- `/login*` : λ€μ νλΌλ―Έν°κ° λΆλ κ²½λ‘λ₯Ό `permitAll()` λ‘ μ€μ ν΄μ£Όμ΄μΌ νλ€.

<br>

### login.html

```html
...
<div th:if="${param.error}" class="form-group">
    <span th:text="${exception}" class="alert alert-danger"></span>
</div>
...
```

- param.error κ°μ΄ `true` λ‘ λμ΄μ€λλ°, νμλ¦¬νμ `th:if` μ μ `true` λ `false` λ‘ λ¬Έμμ΄μ΄ λμ΄μ€λ©΄ μλμΌλ‘ `boolean` μΌλ‘ μΈμνλ€. 
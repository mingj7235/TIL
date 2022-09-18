# 사용자 정의 보안 기능 구현


```yaml

#상속 관계

SecurityConfig (사용자 정의 보안 설정 클래스)
    |
    | [상속]
    V
WebSecurityConfigurerAdapter (스프링 시큐리티의 웹 보안 기능 초기화 및 설정)
    |
    | [상속]
    V
HttpSecurity (세부적인 보안 기능을 설정할 수 있는 API 제공)
    |       |
    v       v
인증 API    인가 API

```




```



### WebSecurityConfigurerAdapter
    - 스프링 시큐리티의 웹 보안 기능 초기화 및 설정
    - ``HttpSecurity`` 클래스를 생성한다.


### HttpSecurity
    - 세부적인 보안 기능을 설정할 수 있는 API 제공

=> 두가지 클래스가 의존성을 추가했을 때, 기본적인 보안 기능을 제공해주는 클래스다. 

<br>

### 인증 API
```java
http.formLogin()
http.logout()
http.csrf()
http.httpBasic()
http.SessionManagement()
http.RemeberMe()
http.ExceptionHandling()
http.addFilter()
```

### 인가 API
```java
http.authorizeRequests()
    .antMatchers()
    .hasRole()
    .permitAll()
    .authenticated()
    .fullyAuthentication()
    .access(hasRole(USER))
    .denyAll()
```

### 💡 코드 구현

```java

@Configuration
@EnableWebSecurity //web 보안을 활성하게 해주는 어노테이션
@RequiredArgsConstructor
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(final HttpSecurity http) throws Exception {
        // configure 메소드를 override 하여 구현 하는 것.


        //인가 정책 설정
        http
                .authorizeRequests()
                    .anyRequest().authenticated(); //모든 요청은 인증을 받아야한다.

        //인증 정책 설정
        http
                .formLogin();
    }
}
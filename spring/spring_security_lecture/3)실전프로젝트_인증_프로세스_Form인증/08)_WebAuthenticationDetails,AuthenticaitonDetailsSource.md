# WebAuthenticationDetails, AuthenticationDetailsSource

# 흐름도
![스크린샷 2022-10-03 오후 1 58 13](https://user-images.githubusercontent.com/74750901/193516673-a9058f41-d606-47d4-904e-d0ce119eb360.png)



- 인증 (로그인 시) `username`, `password` 말고 추가 적인 정보를 필요할 때.

- `Authentication` 에는 ID, PW 가 저장이되고, 별도로 `details` 를 저장한다.


<br>

## WebAuthenticationDetails

- 인증 과정 중 전달된 데이터들을 저장

- `Authentication` 의 `details` 속성에 저장

- `WebAuthenticationDetails` 객체는 `Authentication` 내부의 `details` 에 저장된다. 

```java

@Getter
public class FormWebAuthenticationDetails extends WebAuthenticationDetails {

    private String secretKey;

    public FormWebAuthenticationDetails(final HttpServletRequest request) {
        super(request);
        secretKey = request.getParameter("secret_key"); 
    }

}
```

- `view` 페이지에서 `secret_key` 로 전달한 값을 인증 시 `authentication` 에 저장


<br>

## AuthenticationDetailsSource

- `WebAuthenticationDetails` 객체를 생성하는 클래스. 

```java
@Component
public class FormAuthenticationDetailsSource implements AuthenticationDetailsSource<HttpServletRequest, WebAuthenticationDetails> {

    @Override
    public WebAuthenticationDetails buildDetails(final HttpServletRequest context) {
        return new FormWebAuthenticationDetails(context);
    }

}
```

- 내가 구현한 `WebAuthenticationDetails` 객체를 만들어서 반환

<br>

## SecurityConfig 에 추가

```java
...

        .and()
                .formLogin()
                .loginPage("/login")
                .loginProcessingUrl("/login_proc")

                .authenticationDetailsSource(authenticationDetailsSource)

                .defaultSuccessUrl("/")
                .permitAll() 
        ;

...


```

- .authenticationDetailsSource(authenticationDetailsSource) 추가

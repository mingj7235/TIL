# CustomAuthenticationProvider

## CustomAuthenticationProvider

```java
@Slf4j
@NoArgsConstructor
public class CustomAuthenticationProvider implements AuthenticationProvider {

    @Autowired
    private UserDetailsService userDetailsService;

    @Autowired
    private PasswordEncoder passwordEncoder;


    @Override
    public Authentication authenticate(final Authentication authentication) throws AuthenticationException {


        String username = authentication.getName();
        String password = (String) authentication.getCredentials();


        AccountContext accountContext = (AccountContext) userDetailsService.loadUserByUsername(username);


        if (!passwordEncoder.matches(password, accountContext.getAccount().getPassword())){
            throw new BadCredentialsException("BadCredentialsException");
        }


        UsernamePasswordAuthenticationToken authenticationToken =
                new UsernamePasswordAuthenticationToken(accountContext.getAccount(), null, accountContext.getAuthorities());

        return authenticationToken;
    }


    @Override
    public boolean supports(final Class<?> authentication) {
        return UsernamePasswordAuthenticationToken.class.isAssignableFrom(authentication);
    }

}

```

- supports(final Class<?> authentication)

    - 전달 받은 `Authentication` 이 `UsernamePasswordAuthenticationToken` 클래스인지 확인한다.

<br>

- authenticate(final Authentication authentication)

    - 실질적 검증을 진행하는 메소드

    - 사용자가 입력한 ID, PW 값이 입력된 `Authentication` 객체를 전달 받는다.

    - ID 검증

        - `userDetailsService.loadUserByUsername(username)`

        - `UserDetailsService` 를 호출하여 검증

    - PW 검증

        - `authentication.getCredentials()` : 사용자가 로그인 시도 시, 입력한 

        - `accountContext.getAccount().getPassword()` : ID 검증 후, 반환 받은 `accountContext` 에서 가져온 `Account` 정보에서 받은 PW 정보. 즉, DB 에 저장되어있는 PW 값.

    - 검증을 모두 마쳤으면 `UsernamePasswordAuthenticationToken` 으로 토큰을 만들어서 `provider` 를 호출한 manager 에게 전달한다. 


# CustomUserDetailsService

## 🍎 AccountContext

```java
@Getter
public class AccountContext extends User {

    private final Account account;

    public AccountContext(Account account, final Collection<? extends GrantedAuthority> authorities) {
        super(account.getUsername(), account.getPassword(), authorities);
        this.account = account;
    }
}

```

- `Spring Security` 의 `User` 를 상속 받는다. 

    - `User` 클래스는 `UserDetails` 의 구현체다. 

- `AccountContext` 의 생성자로 `Account` 계정을 받고, 부모 생성자에 `username` 과 `password`, 그리고 `authroities` 를 넘겨준다.

<br>

## 🍉 CustomeUserDetailsService

```java
@Service
@RequiredArgsConstructor
public class CustomUserDetailsService implements UserDetailsService {

    private final UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(final String username) throws UsernameNotFoundException {

        Account account = userRepository.findByUsername(username);

        if (account == null) {
            throw new UsernameNotFoundException("UsernameNotFoundException");
        }

        List<GrantedAuthority> roles = new ArrayList<>();
        roles.add(new SimpleGrantedAuthority(account.getRole()));

        AccountContext accountContext = new AccountContext(account, roles);

        return accountContext;
    }


```

- `UserDetailsService` 를 구현한다.

- 반환값은 `UserDetails` 인데, 앞에서 구현한 `AccountContext` 를 반환 시킨다.


## 🍐 SecurityConfig

```java


    private final UserDetailsService userDetailsService;

    @Override
    protected void configure(final AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService);
    }

```

- `UserDetailsService` 를 주입한다.

- `CustomeUserDetailsService` 에서 `UserDetailsService` 를 구현하였고, 앞으로 이 `Config` 로 인해 내가 만든 `UserDetailsService` 를 사용한다. 
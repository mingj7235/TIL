# CustomUserDetailsService

## π AccountContext

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

- `Spring Security` μ `User` λ₯Ό μμ λ°λλ€. 

    - `User` ν΄λμ€λ `UserDetails` μ κ΅¬νμ²΄λ€. 

- `AccountContext` μ μμ±μλ‘ `Account` κ³μ μ λ°κ³ , λΆλͺ¨ μμ±μμ `username` κ³Ό `password`, κ·Έλ¦¬κ³  `authroities` λ₯Ό λκ²¨μ€λ€.

<br>

## π CustomeUserDetailsService

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

- `UserDetailsService` λ₯Ό κ΅¬ννλ€.

- λ°νκ°μ `UserDetails` μΈλ°, μμμ κ΅¬νν `AccountContext` λ₯Ό λ°ν μν¨λ€.


## π SecurityConfig

```java


    private final UserDetailsService userDetailsService;

    @Override
    protected void configure(final AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService);
    }

```

- `UserDetailsService` λ₯Ό μ£Όμνλ€.

- `CustomeUserDetailsService` μμ `UserDetailsService` λ₯Ό κ΅¬ννμκ³ , μμΌλ‘ μ΄ `Config` λ‘ μΈν΄ λ΄κ° λ§λ  `UserDetailsService` λ₯Ό μ¬μ©νλ€. 
# WebIgnore 

π‘ ``js / css / image `` νμΌλ± λ³΄μ νν°λ₯Ό μ μ©ν  νμκ° μλ λ¦¬μμ€λ₯Ό μ€μ 

```java
    @Override
    public void configure(final WebSecurity web) throws Exception {
        
        web
            .ignoring()
                .requestMatchers(
                    PathRequest
                        .toStaticResources()
                            .atCommonLocations()
                                );
        
    }
```

- Spring Security λ λͺ¨λ  μμλ€μ λ³΄μ κ²μ¬λ₯Ό νλλ°, λ³΄μ νν°λ₯Ό μ μ©ν  νμ μλ μ μ  μμλ€μ κ±΄λ λΈ μ μλλ‘ μ€μ  νλ κ²μ. 


<br>

`permitAll()` κ³Όμ μ°¨μ΄μ μ λ¬΄μμΈκ°? 

```java
 @Override
    protected void configure(final HttpSecurity http) throws Exception {
        http
                .authorizeRequests()
                    .antMatchers("/", "/user").permitAll()

        .and()
                .formLogin()
        ;
    }
```

- `permitAll()` μ λ³΄μ νν°μ κ²μ¬λ λ°λ κ²μ΄λ€. λ³΄μ νν°λ₯Ό κ±°μΉκ² λλ€..

- `webIgnore` λ μμ λ³΄μ νν°λ₯Ό κ±°μΉμ§ μκ² νκΈ° λλ¬Έμ, μμμ μ¬μ©μ΄ ν¨μ¬ μ λ€.
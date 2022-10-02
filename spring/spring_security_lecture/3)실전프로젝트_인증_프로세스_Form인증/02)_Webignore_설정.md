# WebIgnore 

💡 ``js / css / image `` 파일등 보안 필터를 적용할 필요가 없는 리소스를 설정

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

- Spring Security 는 모든 자원들을 보안 검사를 하는데, 보안 필터를 적용할 필요 없는 정적 자원들을 건너 뛸 수 있도록 설정 하는 것임. 


<br>

`permitAll()` 과의 차이점은 무엇인가? 

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

- `permitAll()` 은 보안 필터에 검사는 받는 것이다. 보안 필터를 거치게 된다..

- `webIgnore` 는 아예 보안 필터를 거치지 않게 하기 때문에, 자원의 사용이 훨씬 적다.
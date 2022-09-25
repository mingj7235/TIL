# 인가 API - 권한 설정 및 표현식


## 인가 API - 권한 설정

- 선언적 방식

    - URL
        ```java
            http.antMatchers("/users/**").hasRole("USER")
        ```
    
    - Method
        ```java
            @PreAuthoize("hasRole('USER')")
            public void user(){
                System.out.println("user")
            }
        ```

- 동적 방식 - DB 연동 프로그래밍

    - URL

    - Method

* ⚠️ 선언적 방식의 Method 방식과 동적 방식의 URL, Method 방식은 실전프로젝트에서 자세히!

<br>

### 💡 선언적 방식 - URL

<br>

## 🤖 API 

```java
    @Override
    protected void configure(final HttpSecurity http) throws Exception {
        http
            .antMatcher ("/shop/**") // 특정한 자원의 경로로 재한 시키는 것. 
                                    //이부분을 생략하면 모든 자원에서 보안검사를 한다.
            .authorizeRequests()
                .antMatchers("/shop/login","/shop/users/**").permitAll()
                .antMatchers("/shop/mypage").hasRole("USER")
                .antMatchers("/shop/admin/pay").access("hasRole('ADMIN')")
                .antMatchers("/shop/admin/**").access("hasRole('ADMIN') or hasRole('SYS')") //spEl 사용
                .anyRequest().authenticated(); // 인증을 받지 못하면 자원에 해당하지 못함

```

- 주의사항 - 설정 시 구체적인 경로가 먼저 오고, 그것보다 큰 범위의 경로가 뒤에 오도록 해야 한다.
        
         a. .antMatchers("/shop/admin/pay")
         b. .antMatchers("/shop/admin/**")

         - 위에서부터 아래로 해석을 한다. 그렇기 때문에 더 구체적인 경로인 a 가 먼저오고, 그 후에 b를 배치해야한다. b가 먼저오게되면, SYS 의 권한을 가진 요청도 pay 에 접근할 수 있게 되는 일이 발생하게 된다. 


## 🌱 표현식

[] 표현식 사진

* anonymous() : 인증된 사용자는 접근이 불가능하다. 왜냐하면, 익명사용자 또한 ``ANONYMOUS`` 라는 ROLE 을 가진 사용자이기 때문이다. 모두 접근하고자 하려면 permitAll()을 사용해야한다.

* hasRole(String) : String 값에 ``ROLE`` prefix 를 제외한 채로 넣어야 한다. ex) hasRole("ADMIN")

* hasAuthority(String) : String 값에 prefix 를 포함한 채 넣어야한다. ex) hasAuthority ("ROLE_ADMIN")



## 🍉 메모리 방식으로 사용자를 생성하는 방법 (테스트용)

```java
        //메모리로 사용자를 임의로 생성하는 방법 (테스트용)
    @Override
    protected void configure(final AuthenticationManagerBuilder auth) throws Exception {

        // password : {noop}은 인코더할 때 프리픽스로 어떻게 할 것인지 패스워드 알고리즘에 보내는 것임. 유형을 적어줘야한다.
        // {noop}은 아무런 인코더를 안쓰고 변화가 없다는 의미임
        auth.inMemoryAuthentication()
            .withUser("user").password("{noop}1111").roles("USER");
        auth.inMemoryAuthentication()
            .withUser("sys").password("{noop}1111").roles("SYS","USER");
        auth.inMemoryAuthentication()
            .withUser("admin").password("{noop}1111").roles("ADMIN","SYS","USER"); 
        
        //role hierarchy 를 안했기에 이렇게 따로 설정해준다.
    }
```

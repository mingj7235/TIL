# UsernamePasswordAuthenticationFilter

## 🌱 UsernamePasswordAuthenticationFilter 의 흐름도

<br>

```
UsernamePasswordAuthenticationFilter    <     -          -            -         |

        |
        |                                                                       |
        v

AntPathRequestMatcher(/login)   - NO - >    chain.doFilter                      |
(URL 요청 정보가 매칭되는지 확인 - loginForm().loginProcessingUrl() 값)
        |
        |                                                                       |
        v

Authentication (Username + Password)                                            |
-> Authentication 객체를 만들어서, username과 Password를 저장함
        |                                                                       |
        |  인증 처리 
        v
                                                                                |
AuthenticationManager   - 위임 - >  AuthenticationProvider - 인증실패 - > AuthenticationException
(인증 처리를 하는 Manager)  < 인증성공 -  (인증처리를 맡아서 하는 클래스)
        |                            인증 성공시 Authentication 객체를 생성하여 저장하고, 다시 AuthenticationManager로 리턴한다.
        |
        v

Authentication (User + Authorities)
최종적인 Authentication 객체를 전달받는데, 인증에 성공한 User 객체와 그객체 부여된 권한인 Authorities 정보를 저장한다.
        |
        |
        v

SecurityContext 에 저장
인증객체를 저장하는 저장소. 후에 Session 에 저장이 되는 클래스. 
        |
        |
        v

SuccessHandler

```

<br>

## 📖 부가 설명

### UsernamePasswordAuthenticationFilter

``UsernamePasswordAuthenticationFilter`` 는 ``AuthenticationManager`` 이 기준 ! 
   
    1.  인증이 완료되기 전 Authentication 객체 (Username + Password)
    2.  AuthenticationManager를 통해 AuthetnciationProvider로 인증이 완료된 후의 Authentication 객체 (User + Authorities)


그 후에 ``SecurityContext`` 에 저장하고, 이 객체는 전역적으로 저장된 객체를 작업할 수 있게 된다. 

### FilterChainProxy

FilterChainProxy : 순서대로 filter 가 돌아갈 수 있도록 해줌. 사용자의 요청을 가장 앞단에서 받아서, 인증 / 인가 처리를 하는 것임. 



# Logout 처리, LogoutFilter

## Logout API

세션 무효화, 인증토큰 삭제, Security Context 삭제, 쿠키정보 삭제, 로그인 페이지로 리다이렉트

```java
    protected void configure (HttpSecurity http) throws Exception {
        http.logout()              
               .logoutUrl("/logout") // 로그아웃 처리 URL 원칙적으로 post 방식으로 logout 처리를 한다.
               .logoutSuccessUrl("/login") // 로그아웃 성공 후 이동페이지
               .deleteCookies("remember-me") // 로그아웃 후 쿠키 삭제 (쿠키 명을 적어준다.)
               .addLogoutHandler(logoutHandler()) // 로그아웃 핸들러
               .logoutSuccessHandler(logoutSuccessHandler()) // 로그아웃 성공 후에 핸들러 ////logoutSuccessUrl과 비슷하지만, handler를 구현하면 더 많은 로직을 안에 넣을 수 있다.
               
    }
```
### logoutUrl ()

원칙적으로 로그아웃은 ``post`` 방식으로 처리한다.


### logoutSuccessUrl() 과 logoutSuccesshandler()

    - logoutSuccessUrl : 단순히 로그아웃 성공 후 이동할 URL

    - logoutSuccessHandler : 더 부가적으로 여러가지 로직을 넣어 줄 수있다.



## LogoutFilter

### 구조

```
REQUEST (post)

        |
        |
        v

LogoutFilter          -          -            -           -     -       -       |

        |
        |                                                                       |
        v

AntPathRequestMatcher(/login)   - NO - >    chain.doFilter                      |
(URL 요청 정보가 매칭되는지 확인 - loginForm().loginProcessingUrl() 값)

        |
        |                                                                       |
        v

Authentication              - - - >     SecurityContext                         |
                                   (현재 로그인 되어있는 인증객체)
        |                                                                       |
        |  인증 처리 
        v
                                                                                |
SecurityContextLogoutHandler                                                    v
                                                                        성공적으로 되었을시 
                                                                   SimpleUrlLogoutSuccessHandler
        |           |                       |
        |           |                       |
        v           v                       v

    세션 무효화     쿠키삭제       SecurityContextHolder.clearContext() 
                                    (SecurityContext 객체 초기화)

```
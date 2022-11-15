# Cookie 삭제 (Domain 이 설정 되어있는 쿠키)


## 개요

- 로그인, 로그아웃을 Spring security, JWT, Redis 를 통해서 하고, 쿠키를 사용하여 토큰을 저장한다.
  
## Trouble

- 로그아웃을 진행하는 로직에 Cookie 를 삭제하려고 하였으나, 삭제되지 않았다.


## Cookie 설정 

```java
 private Cookie getLoginCookie (final @NotNull TokenDto tokenDto) {
        Cookie cookie = new Cookie (
                AUTHORIZATION,
                tokenDto.getGrantType() + tokenDto.getRefreshToken()
        );
        cookie.setPath("/");
        cookie.setMaxAge(60 * 60 * 24);
        cookie.setDomain(".xxx.com");
        return cookie;
    }
```

- Domain 공유를 위해 설정을 해주었다.

## Cookie 삭제 (변경 전)

```java
    public Cookie logout () {
        Cookie cookie = new Cookie(AUTHORIZATION, "");
        cookie.setPath("/");
        cookie.setMaxAge(0);
        return cookie;
    }
```

## Cookie 삭제 (변경 후)

```java
    public Cookie logout () {
        Cookie cookie = new Cookie(AUTHORIZATION, "");
        cookie.setPath("/");
        cookie.setDomain(".xxx.com");
        cookie.setMaxAge(0);
        return cookie;
    }
```

## 결론

- Domain 을 설정한 Cookie 를 지우기 위해서는 동일하게 Domain 을 설정 하여 덮어씌워줘야한다.
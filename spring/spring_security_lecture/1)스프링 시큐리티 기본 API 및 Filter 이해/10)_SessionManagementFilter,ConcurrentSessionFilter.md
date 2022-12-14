# SessionManagementFilter, ConcurrentSessionFilter


## ๐ SessionManagementFilter

- ์ธ์๊ด๋ฆฌ

    - ์ธ์ฆ ์ ์ฌ์ฉ์์ ์ธ์์ ๋ณด๋ฅผ ๋ฑ๋ก, ์กฐํ, ์ญ์  ๋ฑ์ ์ธ์ ์ด๋ ฅ์ ๊ด๋ฆฌ

- ๋์์  ์ธ์ ์ ์ด (ConcurrentSessionFilter ์ ๋ง๋ฌผ๋ ค์ ์ ์ดํจ)

    - ๋์ผ ๊ณ์ ์ผ๋ก ์ ์์ด ํ์ฉ๋๋ ์ต๋ ์ธ์์๋ฅผ ์ ํ

- ์ธ์ ๊ณ ์  ๋ณดํธ

    - ์ธ์ฆํ  ๋๋ง๋ค ์ธ์์ฟ ํค๋ฅผ ์๋ก ๋ฐ๊ธํ์ฌ ๊ณต๊ฒฉ์์ ์ฟ ํค ์กฐ์์ ๋ฐฉ์ง

- ์ธ์ ์์ฑ ์ ์ฑ

    - Always, If_Required, Never, Stateless

<br>

## ๐ ConcurrentSessionFilter

- ๋งค ์์ฒญ๋ง๋ค ํ์ฌ ์ฌ์ฉ์์ ์ธ์ ๋ง๋ฃ ์ฌ๋ถ๋ฅผ ์ฒดํฌ -> ๋์์  ์ธ์ ์ฒ๋ฆฌ 

- ์ธ์์ด ๋ง๋ฃ๋์์ ๊ฒฝ์ฐ ์ฆ์ ๋ง๋ฃ ์ฒ๋ฆฌ๋ฅผ ํจ. 

```java
    session.isExpired() == true // ์ด ๊ฒฝ์ฐ๊ฐ ์ธ์์ด ๋ง๋ฃ๋์๋ค๋ ์๋ฏธ

    // ๋ง๋ฃ๋์์ ๋
    // - ์ฆ์ ๋ก๊ทธ์์ ์ฒ๋ฆฌ
    // - ๋ก๊ทธ์์ ์ฒ๋ฆฌ ํ ์ฆ์ ์ค๋ฅ ํ์ด์ง ์๋ต "This session has been expired"
```

## ๐ ๋์ ์๋ฆฌ

![แแณแแณแแตแซแแฃแบ 2022-09-22 แแฉแแฎ 11 15 45](https://user-images.githubusercontent.com/74750901/191783426-978ad785-d7a3-456f-942f-63af9750bdd5.png)
<i>์ถ์ฒ : ์ ์์๋ ๊ฐ์ 1-10) ์ธ์ ์ ์ด ํํฐ : SessionManagementFilter, Concurrent</i>


``SessionManagementFilter``, ``ConcurrentSessionFilter`` ๊ฐ ๋ง๋ฌผ๋ ค์ ๋์์  ์ธ์์ ์ด๋ฅผ ํจ


![แแณแแณแแตแซแแฃแบ 2022-09-22 แแฉแแฎ 11 18 51](https://user-images.githubusercontent.com/74750901/191783459-6358f903-3e10-40f1-a582-3c0d3426b42d.png)
<i>์ถ์ฒ : ์ ์์๋ ๊ฐ์ 1-10) ์ธ์ ์ ์ด ํํฐ : SessionManagementFilter, Concurrent</i>

SessionManagementFilter

    - ConcurrentSessionControlAuthenticationStrategy

    - ChangeSessionIdAuthenticationStrategy

    - RegisterSessionAuthenticationStrategy

## ์ํฉ

``๐๐ปโโ๏ธ User 1`` ์ด ๋ก๊ทธ์ธ ์๋

1. ``ConcorrentSessionControlAuthenticationStrategy``
    
    - ``session`` ์ count ํ๋ค. ์ต๋ ํ์ฉ ๊ฐ์๋ฅผ ๋น๊ตํ์ฌ ๋์ ์ ์ session ์ ์ ์ดํ๋ค.
     

2. ``ChangeSessionIdAuthenticationStrategy``

    - ์ธ์ ๊ณ ์ ๋ณดํธ ํ๋ ํด๋์ค (session.changeSessionId())

3. ``RegisterSessionAuthenticationStrategy``

    - ์ฌ์ฉ์์ ์ธ์ ์ ๋ณด๋ฅผ ๋ฑ๋กํ๋ ํด๋์ค

    - ์ด ํด๋์ค๋ฅผ ํตํด Session ์ ๋ฑ๋กํ๊ธฐ ๋๋ฌธ์, ์ด๋ session count ๊ฐ 1์ด๋๋ค.

<br>

``๐๐ป User 2``๊ฐ ``๐๐ปโโ๏ธ User 1``๊ณผ ๋์ผํ ๊ณ์ ์ผ๋ก ๋ก๊ทธ์ธ ์๋ 

1. ``sessionCount == maxSessions`` ์ผ ๊ฒฝ์ฐ 
    - ์ธ์ฆ ์คํจ ์ ๋ต์ธ ๊ฒฝ์ฐ 
        -> User2 ์๊ฒ ์ธ์ฆ ์คํจ ์์ธ (``SessionAuthenticationException``) 

    - ์ธ์ ๋ง๋ฃ ์ ๋ต์ธ๊ฒฝ์ฐ
        -> User1์ ์ธ์์ ๋ง๋ฃ์ํจ๋ค. ``session.expireNow()``
        -> User2 ๋ ์ธ์ฆ์ ์ฑ๊ณตํ๋ค. 

        -> ์ด ๊ฒฝ์ฐ, ``๐๐ปโโ๏ธ User 1`` ์ด ๋ค์ ๋ก๊ทธ์ธํ  ๋, ``ConcurrentSessionFilter``๊ฐ ์ฌ์ฉ์์ ์ธ์๋ง๋ฃ๋ฅผ ๊ณ์ ์ฒดํฌํ๊ณ , ์ด Filter๋ฅผ ํตํด ``session.isExpired()``๊ฐ ๊ฐ์ง๋์ด Logout ์ด ๋๊ณ , ์ธ์ ๋ง๋ฃ ์๋ต์ ๋ฐ๋๋ค. 

        * ``ConcurrentSessionFilter`` ๋ ๊ณ์ ์ธ์ ๋ง๋ฃ๋ฅผ ์์ฒญ๋ง๋ค ์ฒดํฌํ๋ค. 


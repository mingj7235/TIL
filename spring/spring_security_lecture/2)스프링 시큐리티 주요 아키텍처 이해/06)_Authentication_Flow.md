# Authentication Flow

## ๐ค ํ๋ฆ๋

์ธ์ฆ์ ํ๋ฆ ! 


### Form ์ธ์ฆ ๋ฐฉ์์ ๋ก๊ทธ์ธ ์์ฒญ

![แแณแแณแแตแซแแฃแบ 2022-10-01 แแฉแแฎ 3 17 10](https://user-images.githubusercontent.com/74750901/193396412-2842a9af-3559-4f50-a4cb-0c6e222b3b8f.png)
<i>์ถ์ฒ : ์ ์์๋ ๊ฐ์ 2-6) Authentication Flow </i>

<br>

- `UsernamePasswordAuthenticationFilter` ๋ `Authentication` ๊ฐ์ฒด์ ์์ฒญ์ผ๋ก ๋ถํฐ ์ ๋ฌ ๋ฐ์ ID ์ PW ๋ฅผ ๋ด์ ํ ํฐ ๊ฐ์ฒด๋ฅผ ์์ฑํ์ฌ `AuthenticationManager` ์ ์ ๋ฌํ๋ค. 

- `AuthenticationManager` ๋ `AuthenticationProvider` ์๊ฒ ์์ํ๋ค.

    - List ์์ ์๋ `AuthenticationProvider` ์ค ํ๋๋ฅผ ์ ํํ์ฌ ์ธ์ธต ์ฒ๋ฆฌ๋ฅผ ์์ํ๋ ์ญํ 

- `AuthenticationProvider`

    - ์ค์ง์ ์ผ๋ก `ID, PW` ๋ฅผ ๊ฒ์ฆํ๋ค.

    - `ID ๋ username` ์ ์๋ฏธํ๋ค. 

    - `UserDetailsService` ์ ID ๋ฅผ ์ ๋ฌํ๋ฉด์, ์ ์  ๊ฐ์ฒด๋ฅผ ์์ฒญํ๋ค. 

        - ์ฌ๊ธฐ์ `username` ์ ์ ๋ฌํ๋ค. ์ฆ, <b>ID ๊ฒ์ฆ์ ํ๋ ๊ฒ. </b>

- `UserDetailsService`

    - `DB`๋ฅผ ํตํด ์ ์  ๊ฐ์ฒด๋ฅผ ์กฐํํ๋ค.

    - ์ ์ ๊ฐ ์๋ค๋ฉด `UserDetails` ํ์์ผ๋ก ์ ์  ๊ฐ์ฒด๋ฅผ ๋ฐํํ๋ค. 

        - <b>UserDetials ํ์์ผ๋ก ๋ฐํ์ ๋ฐ์๋ค๋ ๊ฒ์ ID ๋ ๊ฒ์ฆ์ด ๋์๋ค๋ ๊ฒ์.</b>

        - ๊ทธ ์ดํ์ `PW` ๋ฅผ ๊ฒ์ฆํ๋ค. 

            - ๋ฐํ ๋ฐ์ `UserDetials ๊ฐ์ฒด์ PW์ Authentication ๊ฐ์ฒด์ ์ ์ฅ๋ ์๋ ฅ๋ฐ์ PW๋ฅผ ๋น๊ต`ํ๋ค.

            - PW ๊ฒ์ฆ์ด ์๋๋ฉด `Bad Credential` ์์ธ๋ฅผ ๋ฐ์ ์ํจ๋ค.

    - ์ ์ ๊ฐ ์๋ค๋ฉด ์๋ค๋ฉด `UsernamePasswordAuthenticationFilter` ๋ฅผ ํตํด ์์ธ๋ฅผ ๋ฐ์ ์ํจ๋ค. 


- ์ต์ข์ ์ผ๋ก ์ธ์ฆ๋ `Authentication` ์ `AuthenticationManager` ๊ฐ `UsernamePasswordAuthenticationFilter` ์ ์ ๋ฌํ๊ณ , ์ด๊ฒ (`Authentication` ์ธ์ฆ๊ฐ์ฒด) ์ `UsernamePasswordAuthenticationFilter` ๊ฐ `SecurityContext` ์ ์ ์ฅํ๋ค. 




    




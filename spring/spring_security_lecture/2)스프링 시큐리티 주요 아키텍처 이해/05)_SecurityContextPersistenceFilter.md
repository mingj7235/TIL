# SecurityContextPersistenceFilter

## ๐ฑ ๊ฐ๋

<b>SecurityContext ๊ฐ์ฒด์ ์์ฑ, ์ ์ฅ, ์กฐํ</b> ์ญํ  ํ๋ Filter 

- ์ต๋ช ์ฌ์ฉ์ ์ ๊ทผ ์

    - ์๋ก์ด ``SecurityContext`` ๊ฐ์ฒด๋ฅผ ์์ฑํ์ฌ `SecurityContextHolder` ์ ์ ์ฅ

    - `AnonymousAuthenticationFilter` ์์ `AnonymousAuthenticationToken` ๊ฐ์ฒด๋ฅผ `SecurityContext` ์ `Authentication`(์ต๋ช ์ฌ์ฉ์ ๊ฐ์ฒด) ์ ์ ์ฅ

- ์ธ์ฆ ์ (์ฌ์ ํ ์ธ์ฆ ๋๊ธฐ ์ ์)

    - ์๋ก์ด `SecurityContext` ๊ฐ์ฒด๋ฅผ ์์ฑํ์ฌ `SecurityContextHolder` ์ ์ ์ฅ

    - `UsernamePasswordAuthenticationFilter` ์์ ์ธ์ฆ ์ฑ๊ณต ํ `SecurityContext` ์ `UsernamePasswordAuthenticationToken` ๊ฐ์ฒด๋ฅผ `SecurityContext` ์ `Authentication` ์ ์ ์ฅ

    - ์ธ์ฆ์ด ์ต์ข ์๋ฃ๋๋ฉด ``Session`` ์ `SecurityContext` ๋ฅผ ์ ์ฅ

- ์ธ์ฆ ํ

    - ์๋ก์ด `SecurityContext` ๋ฅผ ์์ฑํ์ง ์๋๋ค.

    - ``Session`` ์์ `SecurityContext` ๊บผ๋ด์ด `SecurityContextHolder` ์ ์ ์ฅ

    - `SecurityContext` ์์ `Authentcation` ๊ฐ์ฒด๊ฐ ์กด์ฌํ๋ฉด ๊ณ์ ์ธ์ฆ์ ์ ์งํ๋ค.

- ์ต์ข ์๋ต ์ ๊ณตํต

    - ``SecurityContextHolder.clearContext()``

        - ์๋ตํ๋ฉด ํญ์ ``SecurityContextHolder`` ์ ์๋ ``SecurityContext`` ๋ฅผ ์ญ์ ํ๋ค.

        - ์? ๋งค ์์ฒญ๋ง๋ค <b> ์ต๋ช์ฌ์ฉ์๋ , ์ธ์ฆ ์์ ์ด๋ , ์ธ์ฆ ํ์ด๋  ``SecurityContext`` ๋ฅผ `SecurityContextHolder`์ ์ ์ฅํ๊ธฐ ๋๋ฌธ์ด๋ค. </b>


- ``SecurityContextPersistenceFilter`` ๋ ๊ฒฐ๊ตญ ``SecurityContext`` ๋ฅผ ``SecurityContextHolder`` ์ ์ ์ฅ์ํค๋ ์ญํ ์ ํ๋ ๊ฒ์ด๋ค.

    - ์ต๋ช์ฌ์ฉ์ ์ ๊ทผ ์, ์ธ์ฆ ์๋ ์๋ก์ด ``SecurityContext`` ๋ฅผ ์์ฑํ์ฌ ``SecurityContextHolder`` ์ ์ ์ฅ

    - ์ธ์ฆ ํ์๋ ``Session`` ์์ `SecurityContext` ๋ฅผ ๊บผ๋ด์ด `SecurityContextHolder` ์ ์ ์ฅ

- `Filter` ์ค์์ ๋๋ฒ์งธ๋ก ์กด์ฌํ๋ ํํฐ๋ค.

## ๐ค ํ๋ฆ๋

![แแณแแณแแตแซแแฃแบ 2022-09-29 แแฉแแฎ 9 51 00](https://user-images.githubusercontent.com/74750901/193040072-c57ee114-9e46-4887-8da4-fcb6878df85d.png)
<i>์ถ์ฒ : ์ ์์๋ ๊ฐ์ 2-5) ์ธ์ฆ ์ ์ฅ์ ํํฐ - SecurityContextPersistenceFilter </i>

- ๋งค ``Request`` ๋ง๋ค ``SecurityContextPersistenceFilter`` ๋ฅผ ๊ฑฐ์น๋ค.

- ์ธ์ฆ ์  YES : ์ต๋ช์ฌ์ฉ์ ์ ๊ทผ or ์ธ์ฆ ์ ๋๊ฐ์ง ๊ฒฝ์ฐ๋ฅผ ์๋ฏธํ๋ค. 

- ์ธ์ฆ ์  NO : ์ธ์ฆ์ด ์ฑ๊ณตํ ํ์ ์ ๊ทผ์ ์๋ฏธํ๋ค. 

![แแณแแณแแตแซแแฃแบ 2022-09-29 แแฉแแฎ 9 56 16](https://user-images.githubusercontent.com/74750901/193040137-45024171-f364-4ae0-b8d1-033b4369e723.png)
<i>์ถ์ฒ : ์ ์์๋ ๊ฐ์ 2-5) ์ธ์ฆ ์ ์ฅ์ ํํฐ - SecurityContextPersistenceFilter </i>

- ์ธ์ฆ์ ๋ฐ๊ธฐ ์ ์ ``SecurityContext`` ๋ ``null`` ์ด๋ค. ``Authentication`` ๊ฐ์ฒด๊ฐ ์๋ค. 

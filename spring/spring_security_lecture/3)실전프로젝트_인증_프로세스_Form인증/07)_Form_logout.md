# Logout

> Thymeleaf ์ฌ์ฉ


## ๐ ๋ก๊ทธ์์ ๋ฐฉ๋ฒ

- `<form>` ํ๊ทธ๋ฅผ ์ฌ์ฉํด์ `POST` ๋ก ์์ฒญ

    - `logoutFilter` ๊ฐ ์๋ํ์ฌ ๋ก๊ทธ์์ ์ฒ๋ฆฌ

- `<a>` ํ๊ทธ๋ฅผ ์ฌ์ฉํด์ `GET` ์ผ๋ก ์์ฒญ 

    - `SecurityContextLogoutHandler` ํ์ฉํ์ฌ ๊ตฌํ ํ ๋ก๊ทธ์์ ์ฒ๋ฆฌ

    - ์ด ํธ๋ค๋ฌ๋ฅผ ํตํด `logoutFilter` ๊ฐ ์๋ํ๋ค.

<br>


## ๐ ์ธ์ฆ ์ฌ๋ถ์ ๋ฐ๋ผ ๋ก๊ทธ์ธ / ๋ก๊ทธ์์ ํํ

- `<li sec:authorize="isAnonymous()"><a th:href="@{/login}>๋ก๊ทธ์ธ</a></li>`
- `<li sec:authorize="isAuthenticated()"><a th:href="@{/logout}>๋ก๊ทธ์์</a></li>`

- Get ๋ฐฉ์์ ๋ก๊ทธ์์ ๊ตฌํ

    ```java
        @GetMapping ("/logout")
        public String logout (HttpServletRequest request, HttpServletResponse response) {
            Authentication auth = SecurityContextHolder.getContext().getAuthentication();
            
            if (auth != null) {
                new SecurityContextLogoutHandler().logout(request, response, auth);
            }
            
            return "redirect:/login";
        }
    ```
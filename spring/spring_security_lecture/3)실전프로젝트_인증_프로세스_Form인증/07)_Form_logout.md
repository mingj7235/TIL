# Logout

> Thymeleaf 사용


## 🍎 로그아웃 방법

- `<form>` 태그를 사용해서 `POST` 로 요청

    - `logoutFilter` 가 작동하여 로그아웃 처리

- `<a>` 태그를 사용해서 `GET` 으로 요청 

    - `SecurityContextLogoutHandler` 활용하여 구현 후 로그아웃 처리

    - 이 핸들러를 통해 `logoutFilter` 가 작동한다.

<br>


## 🍊 인증 여부에 따라 로그인 / 로그아웃 표현

- `<li sec:authorize="isAnonymous()"><a th:href="@{/login}>로그인</a></li>`
- `<li sec:authorize="isAuthenticated()"><a th:href="@{/logout}>로그아웃</a></li>`

- Get 방식의 로그아웃 구현

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
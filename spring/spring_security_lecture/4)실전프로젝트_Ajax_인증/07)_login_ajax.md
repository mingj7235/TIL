# 로그인 Ajax 구현 & CSRF

## 헤더설정

- 전송 방식이 Ajax 인지의 여부를 위한 헤더 설정

    - `xhr.setRequestHeader("X-Requested-With", "XMLHttpRequest")`

<br>

- CSRF 헤더 설정

    ```html
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">

    <meta id="_csrf" name="_csrf" th:content="${_csrf.token}"/>
    <meta id="_csrf_header" name="_csrf_header" th:content="${_csrf.headerName}"/>

    <head th:replace="layout/header::userHead"></head>
    <script>
        function formLogin(e) {

            var username = $("input[name='username']").val().trim();
            var password = $("input[name='password']").val().trim();
            var data = {"username" : username, "password" : password};

            var csrfHeader = $('meta[name="_csrf_header"]').attr('content')
            var csrfToken = $('meta[name="_csrf"]').attr('content')

            $.ajax({
                type: "post",
                url: "/api/login",
                data: JSON.stringify(data),
                dataType: "json",
                beforeSend : function(xhr){
                    xhr.setRequestHeader(csrfHeader, csrfToken);
                    xhr.setRequestHeader("X-Requested-With", "XMLHttpRequest");
                    xhr.setRequestHeader("Content-type","application/json");
                },
                success: function (data) {
                    console.log(data);
                    window.location = '/';
                },
                error : function(xhr, status, error) {
                    console.log(error);
                    window.location = '/login?error=true&exception=' + xhr.responseText;
                }
            });
        }
    </script>
    ```

    - Ajax로 하면 직접 만들어서 CSRF 를 넣어줘야 한다.

        - 본래는 Thymeleaf 가 만들어주지만 Ajax 통신일 때는 넣어줘야한다.





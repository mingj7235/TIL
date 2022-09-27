# Authentication

## 🙋🏻‍♂️ Authentication - 인증, 당신이 누구인지 증명 하는것 !

<br>

- 사용자의 인증 정보를 저장하는 ``Token`` 개념

- <b>인증 시</b>, ``[id 와 pw]`` 를 담고 인증 검증을 위해 전달 되어 사용됨

- <b>인증 후</b>, ``[최종 인증 결과 (user 객체, 권한 정보)]`` 를 담고 ``SecurityContext`` 에 저장되어 <b>전역적으로</b> 참조가 가능하다.

    ```java
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication()
    ```

- 구조

    - principal : 사용자 아이디 or User 객체를 저장 (Object 객체)

    - credentials : 사용자 비밀번호 (자격 증명)

    - authoritieis : 인증된 사용자의 권한 목록

    - details : 인증 부가 정보

    - Authenticated : 인증 여부 (boolean)

    💡 ``Authentication`` 는 ``Interface`` 다. 

<br>

## 🤖 Authentication 흐름

[] 사진

- ``AuthenticationManager`` 를 기준으로 인증 시, 인증 후가 나뉜다. 

- 인증 시, 인증 후 ``Principal`` 에 담기는 것이 다르다. 

- ``AuthenticationManager`` 를 통해 인증에 성공하게 되면, ``Authentication`` 객체에 정보를 최종적으로 담는다. 

- 그 후에 ``SecurityContext`` 에 저장하여, 인증 객체를 전역적으로 사용하게 된다. 





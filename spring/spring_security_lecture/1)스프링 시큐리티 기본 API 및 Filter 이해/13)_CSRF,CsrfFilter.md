# CSRF, CsrfFilter

인증 (Authentication) 프로세스 구현

[] 상황그림 1

CSRF - 사이트간 요청 위조

Spring Security 는 이러한 CSRF 를 막기 위해 CsrfFilter 를 사용한다.

## Form 인증 - CsrfFilter

- 모든 요청에 랜덤하게 생성된 토큰을 HTTP 파라미터로 요구

    - 클라이언트가 이 발급한 토큰을 반드시 가져와야 한다.

- 요청 시 전달되는 토큰 값과 서버에 저장된 실제 값과 비교한 후 만약 일치하지 않으면 요청은 실패한다.


[] 사진 2 

- Client 가 Form 으로 csrf token 을 담아서 보내야한다. 

- Spring Security 에서는 기본적으로 csrf() 가 활성화 되어있다. 


* Thymeleaf 에서는 자동적으로 Form tag 에 csrf token을 전달해준다. 
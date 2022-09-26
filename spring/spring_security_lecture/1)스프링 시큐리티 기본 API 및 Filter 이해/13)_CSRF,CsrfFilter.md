# CSRF, CsrfFilter

인증 (Authentication) 프로세스 구현

![스크린샷 2022-09-26 오후 9 43 04](https://user-images.githubusercontent.com/74750901/192282981-678db5cf-a0a3-454e-830f-09fb235d4938.png)
<i>출처 : 정수원님 강의 1-13) 사이트 간 요청 위조 - CSRF, CsrfFilter </i>


CSRF - 사이트간 요청 위조

Spring Security 는 이러한 CSRF 를 막기 위해 CsrfFilter 를 사용한다.

## Form 인증 - CsrfFilter

- 모든 요청에 랜덤하게 생성된 토큰을 HTTP 파라미터로 요구

    - 클라이언트가 이 발급한 토큰을 반드시 가져와야 한다.

- 요청 시 전달되는 토큰 값과 서버에 저장된 실제 값과 비교한 후 만약 일치하지 않으면 요청은 실패한다.

![스크린샷 2022-09-26 오후 9 47 05](https://user-images.githubusercontent.com/74750901/192283004-38f75093-24eb-443b-b5e4-8efb01e383e8.png)
<i>출처 : 정수원님 강의 1-13) 사이트 간 요청 위조 - CSRF, CsrfFilter </i>


- Client 가 Form 으로 csrf token 을 담아서 보내야한다. 

- Spring Security 에서는 기본적으로 csrf() 가 활성화 되어있다. 


* Thymeleaf 에서는 자동적으로 Form tag 에 csrf token을 전달해준다. 

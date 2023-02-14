
### 전체적인 플로우

> 1. Web Client 가 Request 를 보낸다.
> 2. Web Container 는 Request 에 알맞는 Web Component 를 선택한다.
> 	1. Web Container 는 Servlet Container, Web Component 는 Servlet
> 3. Web Conponent (Servlet) 은 해당 요청을 할당 받아 작업을 한다.
> 4. 다히 Web Client 에 Response 한다. 


### 요청과 응답

HTTP 는 요청과 응답의 표준 프로토콜을 의미한다.

Request
	- Request Line : Method, Path, HTTP Version
		- Method : 어떤 Method 로 요청할것인지 (Get, Post ..)
		- Path : 호스트와 포트를 제외한 경로부분, 파라미터
		- Http version : 버전
	- Headers
	- Message Body

```http
GET /hello?name=Spring HTTP/1.1

Accept: */*
Accept-Encoding: gzip, deflate
Connection: keep-alive
Host: localhost:8080
User-Agent: HTTPie/3.2.1
```


Response
	- Status Line : HTTP Version, Status Code, Status Text
		- Http version : 이게 제일 앞에 나옴
		- Status Code : 상태 코드 값
		- Status Text : 상태를 설명하는 텍스트
	- Headers
		- content-type 정보는 매우 중요하다.
	- Message Body

```http
HTTP/1.1 200 

Connection: keep-alive
Content-Length: 12
Content-Type: text/plain;charset=UTF-8
Date: Tue, 14 Feb 2023 13:50:30 GMT
Keep-Alive: timeout=60

Hello Spring

```
# CloudFront

## 정의

- 정적, 동적, 실시간 웹사이트 컨텐츠를 유저들에게 전달

- `Edge Location` 을 사용

- 컨텐트 딜리버리 네트워크 (Content Delivery Network. aka CDN)

    - 웹페이지가 불러오는 지역이 어디인지 파악해서 네트워킹 하는것

    - html, js, img 파일등을 가져오는 속도를 cdn 을 통해 비약적으로 상승 가능

    - 웹사이트 호스팅에 먼 지역일 수록 latency 가 많이 생긴다. 즉, content 가 delivery 되는 것이 느려지는데, CDN 은 전 세계에 있는 `Edge Location` 을 통해 content 를 delivery 하는 것.

    - `Edge Location` 은 컨텐츠 정보를 cache 에 저장 하여, 유저들의 지역에 따라 요청에 응답함

- 분산 네트워크 (Distributed Network)

<br>

## 용어 정리

- Edge Location : 컨텐츠들이 캐시에 보관되어지는 장소

- Origin : 원래 컨텐츠들이 들어 있는 곳. 즉, 웹서버 호스팅이 되어지는 곳. 

    - S3, EC2 인스턴스가 오리진이 될 수 있음 

- Distribution : CDN 에서 사용되어지며 Edge Location 들을 묶고 있다는 개념




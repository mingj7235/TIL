# ELB

## 정의
- ELB : Elastic Load Balancers

- 수많은 서버의 흐름을 균형있게 흘려보내는데 중추적인 역할을 함

    - 균형있는 배분. 

- 하나의 서버로 traffic이 몰리는 병목현상 (bottleneck) 방지

- Traffic 의 흐름을 Unhealthy instance -> healthy instance 로 보내준다.

<br>

## ELB 의 종류

1. Application Load Balancer 

- `OSI Layer7` 에서 작동됨

    - web service 의 가장 바깥 부분인 layer 라는 의미. 

- `HTTP, HTTPS 와 같은 traffic 의 load balancing` 에 가장 적합 함

- `고급 request 라우팅 설정을 통하여 특정 서버로 request` 를 보낼 수 있음 

    - 달리 말해 root 를 변경할 수 있다는 의미

<br>

2. Network Load Balancer 

- `OSI Layer 4` 에서 작동됨  

    - `매우 빠른 속도를 자랑`하며, Production 환경에서 종종 쓰인다. 

    - OSI 4 는 transport layer 라고도 불림

- 극도의 performance 가 요구되는 ```TCP traffic``` 에서 적합하다. 

- 초당 수백만개의 request 를 아주 미세한 delay 로 처리 가능하다.

<br>

3. Classic Load Balancer 

- 현재 Legacy 로 간주된다. 즉, 거의 쓰이지 않는다. 

- Layer 7 의 HTTP/HTTPS 라우팅 기능 지원

- Layer 4 의 TCP traffic 라우팅 기능도 지원 

<br>

## Error

Load Balancer Error : 504 Error 

<br>

## X-Forwarded-For 헤더

--

EC2 는 Private IP address 밖에 볼 수가 없다. 

이럴 때, `X-Forwarded-For 헤더`를 통해, `public IP address` 를 찾을 수 있다.


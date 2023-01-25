# ENI - Elastic Network Interface 

## 개념

- `가상 네트워크 인터페이스`

- EC2 와 연결된 가상 네트워크. `인스턴스에 연결되어 네트워크 통신`을 한다. 

- IP주소, MAC 주소 등이 부여된다. 

- 인스턴스 생성시 기본 네트워크 인터페이스가 IP 주소 등의 정보 할당과 함께 생성됨

- EC2 에 추가로 `여러개의 네트워크 인터페이스 연결이 가능`하다. 
    - 서로 다른 IP 주소를 가지고, 서로 다른 라우팅도 가능하다.

<br>

## AWS 콘솔에서 확인

![스크린샷 2023-01-25 오후 9 29 07](https://user-images.githubusercontent.com/74750901/214565372-2498ad99-0f51-4240-9ed2-da79078895cc.png)

<br>

## ENI 생성

![스크린샷 2023-01-25 오후 9 36 13](https://user-images.githubusercontent.com/74750901/214565438-0a4dd904-ed5c-4a10-8fc9-2d67e81fcdc0.png)

![스크린샷 2023-01-25 오후 9 36 34](https://user-images.githubusercontent.com/74750901/214565452-213e6ad4-280c-4a1f-b498-310e4da5c4a0.png)


EC2 에 또다른 ENI 를 연결하려면, 동일 EC2 에 존재할 ENI 와 가용영역 (AZ) 가 동일해야한다. 

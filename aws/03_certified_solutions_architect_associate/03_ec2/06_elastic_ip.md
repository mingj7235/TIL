# Elastic IP (탄력적 IP)

## Public IP vs Private IP

- Public IP : 인터넷 연결에 사용하는 IP

- Private IP : 회사나 집의 내부에서만 사용하는 IP
    - 직접적으로 인터넷 연결이 안되며 인터넷 게이트웨이를 통해야 한다.
    - 인터넷 게이트웨이는 인터넷으로 연결을 해야하므로 Public IP 가 있다.
    - ![스크린샷 2023-01-27 오후 4 55 47](https://user-images.githubusercontent.com/74750901/215037762-7e20d007-af14-407b-b112-7db3a1cc15c1.png)


<br>

## Elastic IP

- 인스턴스 생성시 Public IP 가 자동으로 할당된다.

- 이 Public IP 는 인스턴스를 재시작 하면 다른 IP 로 재할당 받아서 Public IP 주소가 변경된다.  

- Elastic IP 는 `인터넷에 연결 가능한 고정적 (정적) 인 Public IP` 주소를 의미한다. 

- EC2 인스턴스의 ENI 에 탄력적 IP 주소를 연결하면, EC2 인스턴스를 다시 시작해도 동일한 IP 주소로 접속 할 수 있다. 




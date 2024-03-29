# Security Group - 보안 그룹

## 개념

- EC2 인스턴스에 대한 인바운드 (들어오는 트래픽), 아웃바운드 (나가는 트래픽) 을 제어하는 가상 방화벽 역할

- EC2 `인스턴스의 ENI `와 연결 된다.
    - ![스크린샷 2023-01-25 오후 9 53 02](https://user-images.githubusercontent.com/74750901/214570725-9f2291bc-d33d-416b-acf1-1365ddfcc20c.png)


- 보안 그룹은 `허용 규칙만 지정 가능`하고 `거부 규칙은 지정 할 수 없음`
    - 즉, 작성되지 않은 규칙은 기본적으로 접근 불가 라는 의미다.

- 보안 그룹은 연결 상태를 추적하는 `상태 저장 방화벽` (Stateful Firewall)

 <br>

 ## 인바운드, 아웃바운드

 - 인바운드 트래픽 : 외부에서 EC2 인스턴스로 `들어오는` 트래픽

 - 아웃바운드 트래픽 : EC2 인스턴스에서 외부로 `나가는` 트래픽

 - 제어규칙
    - 트래픽 유형 (ex> SSH, HTTP 등)
    - 프로토콜 (트래픽에 따른 프로토콜 ex> TCP, UDP 등)
    - 포트 범위 (ex> SSH - 22, HTTP - 80, HTTPS - 443, MYSQL - 3306 등)
    - 대상
        - 개별 IP주소, CIDR Block, Security Group, PrefixList (IP 목록을 모아놓은 리스트)

<br>

## 상태 저장 (Stateful)

- `아웃바운드 규칙에 상관 없이, 허용된 인바운드 트래픽에 대한 반응으로 외부로 나가는 흐름이 수행`

    - 즉, 인바운드에서 허용한 트래픽은 특별히 아웃바운드에서 정하지 않아도 아웃바운드 트래픽 제약이 없다.
    - ex> 인바운드로 80번 포트를 뚫어줬다면, 아웃바운드 80을 허용하지 않아도 인스턴스에서 80번 포트로부터 아웃바운드 트래픽, 즉 응답을 할 수 있다. 
    
    ![스크린샷 2023-01-25 오후 9 50 00](https://user-images.githubusercontent.com/74750901/214570805-f606dfd4-83b2-44e7-a186-1682864629e0.png)


- 사용자가 인스턴스에서 요청을 전송하면 해당 요청의 응답 트래픽(아웃바운드 트래픽) 은 인바운드 보안 그룹 규칙에 관계없이 인바운드 흐름이 허용된다. 

- 반대도 가능하다 (아웃바운드에서 80 포트에 대해 규칙을 생성했으면 인바운드 없어도 가능)


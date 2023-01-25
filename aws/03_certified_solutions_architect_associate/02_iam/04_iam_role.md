# IAM - Role

## 개념

- AWS 리소스에서 사용하는 자격 증명

- 특정 `AWS 서비스`가 `다른 AWS 서비스에 액세스 하여 작업을 수행할 때 필요`한 권한

- 정책을 연결하여 IAM 역할에 작업 수행에 필요한 권한을 부여

- IAM ROLE 이 서비스에 연결이 되어야 다른 서비스 엑세스가 가능하다

    - ex> EC2 에서 실행되는 WEB 서비스가 S3와 RDS 액세스 권한이 필요할 때 해당 Role 이 있어야한다. 

<br>

## 신뢰 정책

- IAM 역할을 사용하여 AWS 계정간 액세스 권한 위임을 하는 기능

- 신뢰 정책 (Trust Policy) 를 사용하여 다른 AWS 계정에 역할을 위임할 수 있음

- 관련 문제

    - photo 



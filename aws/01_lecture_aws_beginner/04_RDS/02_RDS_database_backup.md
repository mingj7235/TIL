# RDS - Database Backup

## 백업의 종류

- Automated Backup

- DB Snapshot

<br>

## Automated Backup (AB) - 자동 백업 

1. Retention Period (1 - 35일) 안의 어떤 시간으로 돌아가게 할 수 있음

    - Point in time 의 개념

2. AB 는 그날 생성된 스냅샷과 Transaction logs (TL) 을 참고함

3. 디폴트로 AB 기능이 설정되어 있으며, 백업 정보는 S3 에 저장

    - RDS 설정 할 때, 기본적으로 설정이 되어 있다. 

4. AB 동안 약간의 I/O suspension 이 존재할 수 있음 -> latency

    - AB 동안 이란 S3 에 저장하는 동안을 의미한다. 어느 정도의 지연이 있음

<br>

## DB Snapshot - 데이터 베이스 스냅샷

1. 주로 사용자에 의해 실행된다. (수동)

2. 원본 RDS 인스턴스를 삭제해도 스냅샷은 S3 에 존재한다. 

    - AB 는 RDS 인스턴스를 삭제하면 사라진다.

<br>

## 데이터 베이스 백업 하는 경우

1. RDS 인스턴스와 RDS 엔드포인트가 아예 다르게 생성된다. 






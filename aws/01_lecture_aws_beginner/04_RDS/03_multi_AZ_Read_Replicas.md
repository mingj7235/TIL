# Multi AZ, Read Replicas

## Multi AZ (Availablility Zone)

- 원래 존재하는 RDS DB 에 무언가 변화가 생길 때, 다른 Availablility Zone 에 똑같은 복제본이 만들어진다.

    - ex) write 가 생기는 경우

    - Synchronize 를 한다는 의미. 동시 다발적으로 일어난다. 

- AWS 에 의해서 자동으로 관리가 이루어진다 (No admin intervention) 

- 원본 RDS DB 에 문제가 생길 시 자동으로 다른 AZ의 복제본이 사용된다.

- Disaster Recovery Only ! 

    - 성능 개선을 위해서 사용되지는 않는다.

    - 따라서 성능 개선을 기대하기 위해서는 Read Replica 가 사용해야 한다.

<br>

## Read Replica

- Production DB 의 읽기 전용 복제본이 생성됨 (select 용)

- 주로 Read-Heavy DB 작업 시 효율성의 극대화를 위해 사용된다. (Scailing 이 주 목적)

- Disaster Recovery 용도가 아니다!

    - 주 목적은 성능 개선을 위해 사용 되는 것 !

- 하나의 RDS 는 최대 5개 Read Replica DB 허용

- Read Replica 의 Read Replica 생성이 가능 (단, Latency 가 발생)

- 각각의 Read Replica 는 자기만의 고유 Endpoint 가 존재한다. 
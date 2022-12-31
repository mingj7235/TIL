# ElasticCache 

## 정의

- 클라우드 내에서 In-memory 캐시를 만들어줌

- 데이터베이스 에서 데이터를 읽어오는 것이 아니라 캐시에서 빠른 속도로 데이터를 읽어옴

- Read-Heavy 어플리케이션에서 상당한 Latency 감소 효과를 누린다.

## 종류

- Memcached

- Redis

## Memcached

- Object 캐시 시스템으로 알려져 있다.

- ElastiCache 는 Memcached 의 프로토콜을 디폴트로 따른다.

- EC2 Auto Scaling 처럼 크기가 커졌다 작아졌다 하는 것이 가능하다.

    - 데이터 처리에 따라 유동적으로 크기를 변화 시킬 수 있다.

- 오픈소스 

- 사용 적합 예 

    - 가장 단순한 캐싱 모델이 필요한 경우

    - Object caching 이 주된 목적인 경우 (map, hashed 같은 경우가 필요 없을 경우)

    - 캐시 크기를 마음대로 scaling 하기를 원하는 경우 


## Redis

- Key - Value , Set, List 와 같은 다양한 형태의 데이터를 In-Memory 에 저장 가능함

- 오픈 소스

- Multi-AZ 지원 (재해 복구가 가능하다는 의미)

- 사용 적합 예 

    - List, Set 과 같은 데이터 셋이 필요한 경우

    - 리더 보드 처럼 데이터 셋의 랭킹을 정렬하는 용도가 필요한 경우

    - Multi AZ 기능이 사용될 필요가 있을 경우



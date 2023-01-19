# Query VS Scan

## 데이터를 불러오는 방법

1. Query

- Primary Key 를 사용하여 데이터 검색

- Query 사용시 모든 데이터 (컬럼)반환

- ProjectionExpression 파라미터
    - 보고싶은 컬럼만 볼 수 있게끔 쿼리를 수정 할 수 있다.
    - 즉, 쿼리된 데이터중에서, 보고싶은 컬럼만 select 하는 기능

<br>

2. Scan

- 모든 데이터를 불러온다. (primary key 를 사용하지 않는다.)

- ProjectionExpression 파라미터

- 데이터를 모두 가지고 온 후에, ProjectionExpression 을 사용하여 원하는 컬럼 조회


<br>

## Query 와 Scan 을 사용하는 시점

- Query 가 Scan 보다 훨씬 효율적임

- 따라서 Query 사용 추천

    - Scan 은 일단 모든 데이터를 가져온 후, 필터링을 통해 추출 하기 때문에 Query 가 효율적
    - 어마어마한 데이터를 가지고 올 수도 있다.

    - '순차적방법' 을 통해 병생 스캔을 하여 Scan 의 기능을 극대화 할 수 도있지만, 잘 안씀

- Scan 을 사용하는 경우
    - Primary key 가 사용되지 않은 Table, 규모가 작은 Table 에서 사용

    - 즉, 규모가 작은 View Table 같은 경우는 Scan 을 사용하는 것이 이득일 때가 있다.
# RDS

## 정의

Relational DB Service : 관계형 데이터 베이스

- RDS 의 종류

    - Microsoft SQL, Oracle, MySQL, Postgre, Aurora, Maria DB

    - Aurora 는 AWS 에서 제공. 프리티어 지원안한다.


<br>

## Data Warehousing 

- Business Intelligence

- 리포트 작성, 데이터 분석시 사용 (Production Database -> Data Warehousing)

- 매우 방대한 분량의 데이터 로드시 사용

<br>

## OLTP vs OLAP

- OLTP : Insert 와 같이 종종 사용되어지는, 혹은 규모가 작은 데이터를 불러올 때 사용되는 SQL 쿼리가 필요할 때 유용하다. 

    - ex) Order # 210 에만 해당 되는 customer 이름, 주소, 시간 정보 insert 하는 경우
    
<br>

- OLAP : 매우 큰 데이터를 불러올 때 사용. 주로 덩치가 큰 select 쿼리가 사용

    - ex) 특정 회사 부서의 net profit, products 를 select 하는 경우

    - transcation 이 사용되지 않는다.
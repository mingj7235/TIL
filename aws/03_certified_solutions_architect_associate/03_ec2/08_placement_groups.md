# 배치 그룹 (Placement Groups)

## 개념

- EC2 인스턴스 배치를 구성하는 방법
    - 인스턴스를 생성하게 되면 AWS 데이터센터에 물리적 서버에 가상머신이 생기게 된다.
    - 이 가상머신을 물리적 어떤 서버에 어떤방식으로 구동할 것인지에 대한 방법

- 3가지 유형의 배치 그룹

    - 클러스터 배치 그룹 : `고성능 네트워크 연결`로 이루어진 인스턴스 묶음
    - 파티션 배치 그룹 : 인스턴스 그룹을 `하드웨어를 공유하지 않는 파티션 단위`로 분할
    - 분산형 배치 그룹 : 인스턴스 그룹을 `별개의 서버 랙 단위`로 분할

<br>

## 클러스터 배치 그룹

photo

- `근접한 서버를 고속 네트워크로 연결`하여 그룹화

    - 기본적으로 EC2 인스턴스를 생성시에는, 랜덤으로 네트워크가 연결되어있다.

- 네트워크 지연시간이 매우 짧다.

- `짧은 대기시간`이 필요한 고성능 컴퓨팅등에 적합하다.

 <br>

 ## 파티션 배치 그룹

photo

 - `하드웨어를 파티션`으로 그룹화

 - 파티션끼리는 `서로 다른 하드웨어` 사용 (하드웨어를 공유하지 않는다.)  
    - 하나의 하드웨어가 장애가 나도, 영향이 없다.   

- 하나의 하드웨어 장애 발생시 다른 하드웨어에 영향이 없음 (파티션간 장애의 영향을 분리가능)

- 하둡 등의 `빅데이터 분산 처리 시스템`에 사용 

<br>

## 분산형 배치그룹

photo

- `같은 서버랙의 서버`를 그룹화

- 서로 다른 분산그룹의 서버는 다른 서버랙에 있음

- `분산그룹끼리 서버랙을 공유하지 않으므로 서버랙에 장애 발생시 안전`하다.

- `매우 중요하고 고가용성이 필요한 어플리케이션`에 적합
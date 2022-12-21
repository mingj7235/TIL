# EBS 란 ?

## 개념

- Elastic Block Storage
- 저장 공간이 생성되어지며 EC2 인스턴스에 부착된다. 
    - 하드디스크로 생각하면 된다.
    - EC2 인스턴스에 부착되는 가상의 하드디스크 라고 생각하면 가장 편하다.

- 디스크 볼륨 위에 File System 이 생성된다.

- EBS 는 특정 Availability Zone 에 생성된다. 즉, region 에 종속된다. 

- Avaliability Zone 이란 

    - Disastor recovery 의 개념으로 매우 중요한 개념이다. 

<br>

## EBS 볼륨 타입 

- SSD 군

1. General Purpose SSD (GP2) : 최대 10k IOPS 를 지원. 1GB 당 3IOPS 속도가 나온다. 

2. Provisioned IOPS SSD (IO1) : 극도의 I/O 률을 요구하는 환경에서 주로 사용. 즉, 매우 큰 DB 관리 환경. 10K 이상의 IOPS 를 지원한다. (but, 가격이 매우 비쌈)


- HDD군 / Magnetic

1. Throughput Optimized HDD (ST1) : 빅데이터 Dataswarhouse, log 프로세싱시 주로 사용. 
but,  boot volume 으로 사용이 불가능하다. (운영체제를 가질 수 없다)

2. CDD HDD (SC1): 파일 서버와 같이 드문 volume 접근 시 주로 사용. 
역시, boot volume 으로 사용 불가능하나 비용은 매우 저렴하다.

3. Magnetic (Standard) : 디스크 1GB 당 가장 싼 비용을 자랑한다. Boot volume 으로 유일하게 사용 가능하다. 


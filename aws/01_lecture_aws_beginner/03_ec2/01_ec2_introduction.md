# EC2 란? 

## 정의 

EC2 : Elastic Compute Cloud

즉, 유연하게 사용량에 따라 서버의 capacity 를 조절할 수 있는 서비스다. 

<br>

## 지불 방법

On-demand : 시간 단위로 가격이 고정

    - 오랜시간 동안 선불을 내지 않고, 최소한의 비용을 지불해서 EC2 인스턴스를 사용하고 싶을 때.
    - 앱 개발 시 최초로 EC2 인스턴스에 deploy 할 때 유용하다.

Reserved : 한정된 EC2 용량 사용 가능

    - 개발의 시작과 끝을 아는 경우 유용하다. 
    - 안정된, 예상 가능한 workload 시 Reserved 사용이 권장된다. 
    - 선불로 인한 할인 적용 

Spot : 입찰 가격이 적용. 가장 큰 할인률. 인스턴스의 시작과 끝 기간이 전혀 중요하지 않을 때 유용

    - 단순히 비용 절감 시 유용. 인스턴스의 시작 / 끝시점에 구애 받지 않을 경우 권장된다. 

<br>

## EC2 를 사용하기 위해 EBS 라는 디스크 볼륨을 요구한다. 

    - EBS 는 가상의 하드 디스크라고 생각하면 편하다.


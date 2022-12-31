# CloudWatch

## 개요

- AWS 리소스 사용의 실시간 모니터링 기능 지원

- 다양한 이벤트들을 수집하여 로그파일로 저장

    - 이벤트란 특정한 상황을 의미한다.

    - ex> S3 버킷 파일 업로드 & 삭제, S3 버켓 접근 시 접근 거부 발생, RDS DB에 접속 시도 등

- 이벤트 & 알람 설정을 통해 SNS, AWS Lambda 로 전송 가능

    - 트리거가 되어 특정 액션을 취하게 할 수 있음 

- [CloudWatch 사용 가능 서비스] : EC2, RDS, S3, ELB, 등등 가능

<br>

## 모니터링 종류

1> Basic Monitoring 

- 무료

- 5분 간격으로 최소의 Metrics 제공

2> Detailed Monitoring

- 유료

- 1 분 간격으로 자세한 Metrics 제공


<br>

## CloudWatch 사용 용례

케이스 1

- Use Case : 매일 얼마나 많은 사용자들이 모바일 앱을 사용하는지 알고 싶음

- Potential Issue : 특정날에 수많은 traffice 이 몰릴 수 있어 병목 현상이 생길 수 있음

- Solution : 메일 traffic rate 와 특정 버튼의 유저 클릭 횟수를 분석하여 더 효율적인 앱개발을 할 수 있는 통찰력을 얻을 수 있음

케이스 2

- Use Case : 특정 시간대에 웹 서버 상태를 점검하여 비용 절감 목표

- Potential Issue : 똑같은 비용을 내며 AWS 리소스들을 사용하지만 낮 시간대와 밤 시간대에 필요한 서버의 성능은 달라질 수 있기 때문에 금전적 손실이 생길 수 있음 (주로 밤 시간대가 낮 시간대보다 서버가 오랫동안 죽어있음)

- Solution : 알람 설정을 통하여 특정 threshold 에 도달했을 때 개발자에게 상황을 보고해줌으로서 서버 management 를 할 수 있음 
# Lambda

## 정의

- Serverless 의 주축을 담당

    - Serverless 란 ? : 클라우드가 직접 서버를 운영하며 리소스를 직접 할당함.

    - 사람이 간섭이 없는 것.

- Events 를 통하여 Lambda 를 실행 시킴

    - ex> S3 에 파일이 업로드 된다. -> '업로드 된다.' 등을 Event 라고한다.

    - 이런 Event 가 발생될 때, Lambda 가 실행 됨.

- NodeJS, Python, Java, GO 등 다양한 언어 지원

    - 즉, 해당 언어로 Lambda 로직을 짤 수 있다.

- Lambda Function

<br>

## 비용

- Lambda Function 이 실행될 때만 돈을 지불함

- 매달 100만 함수 호출 시 무료로 Lambda 가 제공된다. (그 이후로는 유료)

<br>

## 기타

- 최대 300초 런타임 시간 허용 (5분)

    - 종종 Timeout 이 나는 이유가 5분의 런타임 시간이 있기 때문이다.

- 512mb 의 일시적인 디스크 공간을 제공 (/tmp/)

    - 주로 데이터 pre-process 실행 시 사용 됨 

- 최대 50mb Deployment Package 제공

    - 로컬에서 작업 후, Deployment 를 통해 함수를 실행 시킬 수도 있다. 

    - 50mb 초과 시, S3 버켓 사용하여 경로를 지정해서 사용 가능하다.


<br>

## 사용 용례

케이스 1>

- S3에 데이터가 업로드 될 때, Lambda를 통해 데이터를 정제하여 DB 에 저장 할 때 사용 가능.

케이스 2>

- IoT 를 통해 실시간으로 데이터가 수집될 때, Lambda 를 통해 데이터의 단위 혹은 형변환을 통해 변경한 후에 SNS 를 통해 반영된 데이터를 고지할 수 있다.



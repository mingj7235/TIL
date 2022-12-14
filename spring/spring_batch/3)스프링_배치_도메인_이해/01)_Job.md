# Job

## 도메인 이해

<br>

### 기본 개념

- 배치 계층 구조에서 `가장 상위에 있는 개념`으로,` 하나의 배치 작업 자체`를 의미한다.

    - "API 서버의 접속 로그 데이터를 통계 서버로 옮기는 배치" 인 `Job 자체를 의미`한다는 것 !

- `Job Configuration` 을 통해 생성되는 객체 단위로서 배치작업을 어떻게 구성하고 실행할 것인지 전체적으로 설정하고 명세해 놓은 객체

- 배치 Job 을 구성하기 위한 최상위 인터페이스이며 스프링 배치가 기본 구현체를 제공한다.

- 여러 Step 을 포함하고 있는 컨테이너로서 `반드시 한개 이상의 Step` 으로 구성해야 한다.

<br>

### 기본 구현체

- 🍎 SimpleJob

    - 순차적으로 Step 을 실행시키는 Job

    - 모든 Job 에서 유용하게 사용할 수 있는 표준 기능을 갖고 있음

    - 가장 기본적인 구현체 (Step 의 순차적으로 구현된다.)

- 🍊 FlowJob

    - 특정한 조건과 흐름에 따라 Step 을 구성하여 실행시키는 Job (순차적이 아니라, 특정한 조건과 흐름이 존재하여 설정과 구성을 달리 할 수 있다. )

    - Flow 객체를 실행시켜서 작업을 진행함. 

<br>

### 도식도

![스크린샷 2022-10-26 오후 10 49 57](https://user-images.githubusercontent.com/74750901/198049466-524d1198-5661-403f-af52-9f0a37a43796.png)



# JobInstance

## 도메인 이해

<br>

### 기본 개념

- Job 이 실행될 때 생성되는 Job 의 논리적 실행 단위 객체로서` 고유하게 식별 가능한 작업 실행`을 나타냄

- Job 의 설정과 구성은 동일하지만 `Job 이 실행되는 시점에 처리하는 내용은 다르기 때문에` Job 의 실행을 구분해야 함 

    - ex> 하루에 한 번 씩 배치 Job 이 실행된다면 매일 실행되는 각각의 Job 을 JobInstance로 표현한다.

- JobInstance 생성 및 실행

    - 처음 시작하는 `Job + JobParameter` 일 경우 새로운 JobInstance 생성

        - JobLanucher 는 두가지 인자를 갖는다. (Job, JobParameter)

    - 이전과 동일한 Job + JobParameter 로 실행할 경우 이미 존재하는 JobInstance 리턴

        - 내부적으로 JobName + jobKey (jobParameters 의 해시값) 을 가지고 JobInstance 객체를 얻는다.
        - 그러므로, jobParameters 는 항상 다른 값이어야햔다. (그래서 실행되는 시간 값을 parameter 로 쓰는 경우가 많음)

- Job 과는 1:M 관계다 

<br>

### BATCH_JOB_INSTANCE 테이블과 매핑

- `JOB_NAME(Job)` 과 `JOB_KEY(JobParameter 해시값)` 이 동일한 데이터는 중복해서 저장 불가

<br>

### 흐름도

![스크린샷 2022-10-27 오후 10 30 25](https://user-images.githubusercontent.com/74750901/198306566-e83c5b55-413b-49ea-bd31-3a78c4446e89.png)



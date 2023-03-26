#batch #jobExecution

## 1. 기본 개념

- JobInstance 에 대한 한번의 시도를 의미하는 객체
- `Job 실행중에 발생한 정보들을 저장하고 있는 객체`를 의미한다.
	- 시작시간, 종료시간, 상태, 종료상태의 속성을 가진다.
<br>

## 2. JobInstance 와의 관계

- JobExecution 은 'FAILED', 'COMPLETED' 등의 Job 의 실행 결과 상태를 가지고 있다.
- JobExectuion 의 실행 상태 결과가 'COMPLETED' 면, JobInstance 실행이 완료된 것으로 간주해서 재실행이 불가하다. 
- JobExectuion 의 실행 상캐 결과가 'FAILED' 면, JobInstance 실행이 완료되지 않은 것으로 간주해서 재실행이 가능하다.
	- JobParameter 가 동일한 값으로 Job 을 실행할지라도 JobInstance 를 계속 실행할 수 있다.
- JobExectuion 의 실행 상태 결과가 'COMPLETED' 될 때까지 하나의 JobInstance 내에서 여러번의 시도가 생길 수 있다.
<br>

## 3. BATCH_JOB_EXECUTION 테이블과 매핑

- JobInstance 와 JobExcution 은 1:M 관게로서 JobInstance 에 대한 성공 / 실패 내역을 가지고 있다. 

#batch #stepExecution

## 1. 기본 개념

- Step 에 대한 한번의 시도를 의미하는 객체로서 Step 실행 중에 발생한 정보들을 저장하고 있는 객체 (StepInstance 는 없음)
	- 시작시간, 종료상태, 상태, commint count, rollback count 등의 속성을 가진다.

- Step 이 매번 시도될 때마다 생성되며 각 step 별로 생성된다.

- Job 이 재시작 하더라도 이미 성공적으로 완료된 Step 은 재실행되지 않고, `실패한 Step 만 실행`된다. 
	- 성공한 Step 은 skip 한다. (기본값)
	- 설정을 통해 Job 이 재시작할 때 성공한 Step 도 재실행 할 수 있다.

- 이전 단계 Step 이 실패해서 현재 Step 을 실행하지 않았다면 StepExecution 을 생성하지 않는다. `Step 이 실제로 시작되었을 때만 StepExecution 을 생성한다.`
	- 실패하더라도 StepExecution 은 생성되지만, 시작되지 않았을 때

- JobExecution 과의 관계 
	- Step 의 StepExecution 이 `모두 정상적으로 완료되어야` JobExecution 이 정상적으로 완료된다.
	- Step 의 StepExecution 중 `하나라도 실패하면 JobExecution 은 실패`한다.
<br>

## 2. BATCH_STEP_EXECUTION 테이블과 매핑

- JobExecution 와 StepExecution 은 1:M 관계
- 하나의 Job 에 여러개의 Step 으로 구성했을 경우 각 StepExecution 은 하나의 JobExecution 을 부모로 가진다.
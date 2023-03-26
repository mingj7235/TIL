#batch #jobParameter

## 1. 기본 개념

- Job 을 실행할 때 함께 포함되어 사용되는 파라미터를 가진 도메인 객체
- 하나의 Job에 존재할 수 있는 여러개의 JobInstane를 구분하기 위한 용도
- JobParameter 와 JobInstance 는 1:1 관계 (유일하다)
<br>

## 2. 생성 및 바인딩

- 어플리케이션 실행 시 주입 하는 방법
	- `java -jar LogBatch.jar {parameterKey1}={parameterValue1} {parameterKey2}={parameterValue2}`
	- ex> java -jar Batch.jar name=user1 seq(long)=2L date(date)=2021/01/01
		- 괄호 안의 long, date 는 타입을 의미한다. 

 - 코드로 생성
	 - `JobParameterBuilder, DefaultJobParametersConverter`

- SpEL 사용
	- `@Value ("#{jobParameter[requestDate]}"), @JobScope, @StepScope 선언`

<br>
## 3. BATCH_JOB_EXECUTION_PARAM 테이블과 매핑

- JOB_EXECUTION 과 1:M 관계 


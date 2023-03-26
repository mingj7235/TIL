#batch #step 

## 1. 기본 개념

- Batch job 을 구성하는 독립적인 하나의 단계.
- 실제 배치 처리를 정의하고 컨트롤하는 데 필요한 모든 정보를 가지고 있는 도메인 객체. 즉, 비지니스 핵심 로직
	- 스탭은 독립적이기에 스캡 끼리의 데이터 간섭이 없다.

- 단순한 단일 테스크 뿐 아니라 입력과 처리 그리고 출력과 관련된 복잡한 비지니스 로직을 포함하는 모든 설정들을 담고 있음

- 배치 작업을 어떻게 구성하고 실행할 것인지 Job 세부 작업을 Task 기반으로 설정하고 명세해 놓은 객체

- 모든 Job 은 하나 이상의 step 으로 구성된다.

- 즉, Batch 의 기술적인 구현은 Step 에 포함을 시킨다.

- Job 은 가장 큰 틀의 명세서고, Job 안에 비지니스 로직을 담지는 않는다. 
	- 실제 비지니스 로직은 Step 에 포함 시키는 것.
<br>
## 2. 기본 구현체

- TaskletStep
	- 가장 기본이 되는 클래스로서 Tasklet 타입의 구현체들을 제어한다.

- PartitionStep
	- 멀티 스레드 방식으로 Step 을 여러 개로 분리해서 실행한다.

- JobStep 
	- Step 내에서 Job 을 실행하도록 한다.
	- Step 안에서 Job 자체를 구성할 수 도 있는 것. 즉, 상위 Job 에서 하위 Job 을 실행하는 것. 즉, 체이닝 식으로 구현할 수 있다.

- FlowStep
	- Step 내에서 Flow 를 실행하도록 한다. 
<br>

## 3. Api 설정에 따른 각 Step 생성


```ad-note
title: TaskletStep - 직접 생성한 Tasklet 실행


~~~java
public class CustomTasklet implements Tasklet {  
  
    @Override  
    public RepeatStatus execute(final StepContribution contribution, final ChunkContext chunkContext) throws Exception {  
  
        System.out.println("step2 was executed by customTasklet");  
  
        return RepeatStatus.FINISHED;  
    }  
  
}
~~~

- custom 으로 만든 tasklet class

<br>

~~~java
@Bean  
public Step step1() {  
    return stepBuilderFactory.get("step1")  
            .tasklet(new CustomTasklet())  
            .build();  
}
~~~

- 직접 Tasklet 을 구현해서 넣을 수 있음

```


```ad-note
title: TaskletStep - ChunkOrientedTasklet 실행

~~~java
@Bean  
public Step chunkOrientedStep () {  
  
    return stepBuilderFactory.get("chunkOriented")  
            .<String, String> chunk(100)  
            .reader(reader())  
            .writer(writer())  
            .build();  
}
~~~

- Spring Batch 가 미리 제공하는 청크지향 Step 
```


```ad-note
title: JobStep 실행

~~~java
@Bean  
public Step jobStep () {  
  
    return stepBuilderFactory.get("jobStep")  
            .job(job())  
            .launcher(jobLoader)  
            .parametersExtractor(jobParameterExtractor())  
            .build();  
}
~~~

- Step 에서 Job 을 실행 
```


```ad-note
title: TaskletStep - FlowStep 실행

~~~java
@Bean  
public Step flowStep () {  
  
    return stepBuilderFactory.get("flowStep")  
            .flow(myFlow())  
            .build();  
}
~~~

- Step 에서 Flow 를 실행 
```


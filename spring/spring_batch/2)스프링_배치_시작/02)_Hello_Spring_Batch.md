# Hello Spring Batch 


## WORK FLOW

![스크린샷 2022-10-21 오후 10 46 43](https://user-images.githubusercontent.com/74750901/197214428-020f484b-0c86-4458-a1a8-37d2b06c90ea.png)


<br>

1. @Configuration 선언

    - 하나의 Batch Job 을 정의하고 Bean 설정

    - Job 을 정의한다는 의미는, Job을 실행하는 각각의 단위들을 모두 정의한다는 의미.

<br>

2. JobBUilderFactory

    - Job 을 생성하는 빌더 팩토리

    - DI 한다. 

<br>

3. StepBuilderFactory

    - Step 을 생성하는 빌더 팩토리

    - DI 한다. 

<br>

4. Job

    - JobBuilderFactory 를 통하여 생성한다. 

<br>

5. Step

    - StepBuilderFactory 를 통하여 생성한다.

<br>

6. Tasklet

    - Step 안에서 단일 태스크로 수행되는 로직 구현

<br>

7. Job 구동 -> Step 실행 -> Tasklet 실행


![스크린샷 2022-10-21 오후 10 48 15](https://user-images.githubusercontent.com/74750901/197214468-2d3066f8-4cf5-4217-a1df-821a0056b9d9.png)




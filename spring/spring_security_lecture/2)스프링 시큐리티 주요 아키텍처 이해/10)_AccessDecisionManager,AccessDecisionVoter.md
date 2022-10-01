# AccessDecisionManager, AccessDecisionVoter

## 🍎 AccessDecisionManager

<br>

### 📚 개념

<br>

-  `인증 정보, 요청 정보, 권한 정보` 를 이용해서 사용자의 자원 접근을 허용 / 거부 를 `최종` 결정 하는 주체

    - `Voter` 들의 반환값을 토대로 최종 결정 한다.

- 여러개의 `Voter` 들을 가질 수 있으며, `Voter` 들로부터 `접근허용, 거부, 보류` 에 해당하는 각각의 값을 리턴받고 판단 및 결정을 한다.

- 최종 접근 거부시 -> `AccessDeniedException` 을 발생시킨다.

<br>

### 💡 접근 결정의 세가지 유형

<br>

👉🏻 `AccessDecisionManager` 는 인터페이스이고, 이걸 접근 결정의 세가지 유형에 맞게 구현한 구현체들이 있다. 

<br>

- AffirmativeBased (Default 값)

    - 여러개의 Voter 클래스 중 하나라도 접근 허가로 결론내면 접근 허가로 판단.

- ConsensusBased

    - 다수표에 의해 최종 결정을 판단

    - 동수일 경우 기본은 `접근허가`

    - `allowIfEqualGrantedDeniedDecisions` 를 `false` 로 설정하면 `접근거부` 로 결정

- UnanimousBased:

    - 모든 `Voter` 가 만장일치로 접근을 승인해야만 `접근허가` 


<br>

## 🍉 AccessDecisionVoter

<br>

### 📚 개념

- 판단을 심사하는 것. 
    
    - 각각의 `Voter` 들이 판단하여 `AccessDecisionMagager` 에게 전달

<br>

- `Voter` 가 권한 부여 과정에서 판단하는 자료 (근거)

    - `Authentication` - 인증 정보 (`user` 객체)

    - `FilterInvocation` - 요청 정보 (`antMatcher("/user")`)

        - 접근하려는 자원 정보 

    - `ConfigAttributes` - 권한 정보 (`hasRole("User")`)

<br>

### 결정 방식

- ACCESS_GRANTED : 접근 허용 (1)

- ACCESS_DENIED : 접근 거부 (-1)

- ACCESS_ABSTAIN : 접근 보류 (0)

    - 접근 보류란, `Voter` 가 해당 타입의 요청에 대해 결정을 내릴 수 없는 경우를 말한다. 

<br>

## 흐름도 

[]

- `FilterSecurityInterceptor` 로부터 시작 


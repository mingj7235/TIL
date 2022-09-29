# SecurityContextHolder, SecurityContext

## 기본 개념

### SecurityContext

- ``Authentication`` 객체가 저장되는 보관소

    - 필요시 언제든지 ``Authentication`` 객체를 꺼내어 쓸 수 있도록 제공되는 클래스

- ``ThreadLocal`` 에 저장되어 아무 곳에서나 참조가 가능하도록 설계되어있음

    - ``Thread`` 는 고유하게 저장한 저장소가 있는데, 그것이 ``ThreadLocal`` 이다.
    
    - 서로 다른 ``Thread`` 는 ``ThreadLocal`` 의 존재로 인해 수정하거나 수정될 수 없다. (꺼내어 올 수만 있다.) ``Thread safe` 하다. 

    - 즉, 그렇기 때문에 ``SecurityContext`` 가 ``ThreadLocal`` 에 저장이 되어 배타적으로 관리되어 안전하며, 전역적으로 꺼내어 사용이 가능하다.

- 인증이 완료되면 ``HttpSession`` 에 저장되어 어플리케이션 전반에 걸쳐 전역적인 참조가 가능하다.


### SecurityContextHolder

- ``SecurityContext`` 를 감싸고 있는 클래스

- ``SecurityContext`` 객체 저장 방식

    - MODE_THREADLOCAL : 스레드당 ``SecurityContext`` 객체를 할당. 기본값임

    - MODE_INHERITABLETHREADLOCAL : 메인 스레드와 자식 스레드에 관하여 동일한 ``SecurityContext`` 를 유지

    - MODE_GLOBAL : 응용 프로그램에서 단 하나의 SecurityContext 를 저장한다.

- ``SecurityContextHolder.clearContext() : SecurityContext`` 기존 정보 초기화


```java
Authentication authentication = SecurityContextHolder.getContext().getAuthentication()
```

## 흐름도

[] 사진

- 인증에 성공한 ``Authentication`` 객체는 ``SecurityContext`` 에 담겨서, ``ThreadLocal`` 에 담기게 된다. 

- 최종적으로 `SPRING_SECURITY_CONTEXT` 라는 이름의 `Session` 에 저장이 된다. 

    ```java
    @GetMapping ("/Thread")
    public String thread (HttpSession session) {

        // HttpSession 을 통해 Authentication 을 가져오는 방법. 
        SecurityContext context 
                = (SecurityContext)session.getAttribute(HttpSessionSecurityContextRepository.SPRING_SECURITY_CONTEXT_KEY);
        Authentication authentication = context.getAuthentication();
        return "Thread";
    }
    ```

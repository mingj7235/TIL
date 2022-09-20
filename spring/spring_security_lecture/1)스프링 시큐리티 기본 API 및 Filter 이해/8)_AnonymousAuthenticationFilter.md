# AnonymousAuthenticationFilter

 ## 구조

![사진](/Users/joshua/Desktop/sc.png)

 - 익명사용자 인증 처리 필터

 - 익명사용자와 인증 사용자를 구분해서 처리하기 위한 용도로 사용

    - 익명 사용자를 단순히 null 로 판단하는 것이 아니라, 따로 별개의 처리를 위해 사용하는 것

 - 화면에서 인증 여부를 구현할 때 ``isAnonymous()`` 와 ``isAuthenticated()`` 로 구분해서 사용

 - 인증 객체를 세션에 저장하지 않는다.


## AunonymousAuthenticationFilter 의 존재 이유

<br>

- ``AnonymousAuthenticationToken`` 즉, 익명 사용자용 Authentication 객체 를 생성한다. (``null`` 이아니다.)

- 이 익명사용자용 ``Authentication`` 을 ``Security Context`` 에 저장한다. 

 - ``Spring Security`` 는 ``AnonymousAuthenticationFilter`` 를 통해 단순히 인증받지 못한 사용자를 null로 두지 않고, 익명사용자로 만들어서 관리한다. 

 - 하지만, 이 ``Authentication`` 객체를 `Session` 에 따로 저장하지는 않는다. 



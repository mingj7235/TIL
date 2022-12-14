# GitHub Actions Pipeline 구축 중 직면한 에러상황 - 1 👻

## 🌱 헤더 정보를 ``root`` 에 push 하고 나서 해당 ``submodule`` 의 내용을 push 하지 않으면 ``submodule recursive`` 할 때 문제가 생긴다.


### 🛠 직면 했던 에러 상황 

 ``web submodule`` 만 ``respository_dispatch`` 를 통한 ``trigger`` 를 발동 시켜 ``deploy`` 시키고 싶었으나, 그 전에 ``root`` 를 push 할 때 다른 ``submodule`` 의 ``head`` 정보를 push 해놓은 상태였다. 

 이 상태에서는 이미 ``root`` 가 알고 있는 ``submodule`` 의 ``head commit`` 정보를 push 시켜주어야 한다. 

 즉, 다른 ``submodule``의 정보를 ``root``가 push 를 통해 변경 사항을 미리 알게 되면, 하나의 ``submodule`` 을 push 이벤트로 ``trigger``를 발동하려고 해도, 다른 ``submodule`` 의 정보를 함께 가지고 오려고 찾기 때문에, ``branch FETCH_HEAD`` 예외가 터지게 된다. 
 

![스크린샷 2022-08-31 오후 2 22 55](https://user-images.githubusercontent.com/74750901/187609306-e128d1c2-c5ec-4d8d-9aec-de3b6944ef6c.png)




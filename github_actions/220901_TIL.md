# GitHub Actions Pipeline 구축 중 직면한 에러상황 - 2 👻


## 🌱  ``root submodule`` 의 ``head`` 정보는 ``branch`` 상관 없이 공유한다.

<br>

### 🛠 직면 했던 에러 상황 

1. ``DEV branch`` 와 ``PRD branch`` 각각 분리하여 ``root submodule`` 을 관리하려고 했다.

2. ``head`` 정보를 공유하되, 각각의 하위 ``submodule`` 에서 각 ``branch`` 별로 WAS 가 적용되어 빌드 될 것이라고 기대했다.

3. 제대로 ``repository_dispatch`` 가 적용되어 ``trigger`` 도 작동하였으나, 정작 서버에는 변경된 코드가 제대로 반영이 되지 않았다. 

<br>

### 💡 트러블 슈팅

1. ``repository_dispatch`` 의 적용은 ``GitHub Repository`` 의 ``default repository`` 의 ``head`` 를 기준으로 적용이 된다. 즉, ``root submodule`` 의 ``PRD branch`` 에서 적용된다는 것

2. ``PRD branch`` 에서 각각의 ``submodule`` 의 ``head`` 정보를 ``push`` 해주고 나서 다시 하위 ``submodule`` 의 ``DEV branch`` 를 ``commit -> push`` 진행해 주니 적용이 되었다.

3. ``Deploy`` 와 ``event trigging`` 의 주체가 ``root`` 에서 각각의 ``submodule`` 로 전환되었음을 잊지 말자
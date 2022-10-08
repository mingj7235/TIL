# RoleHierarchy

## 계층 권한

![스크린샷 2022-10-08 오후 11 03 15](https://user-images.githubusercontent.com/74750901/194718333-d7900bf4-4ac2-46fe-a398-ae5bb6e2e875.png)

- 각각의 권한은 본래 `단순한 문자열` 로 인식할 뿐이다. 

- `의미있는` 계층관계로 `Role` 의 계층관계를 주기 위해서 `RoleHierarchy` 를 사용해야한다.

- `RoleHierarchyVoter`

    - 인가 심사를 할 때 사용되는 `Voter` 의 종류중 하나이다. 

- DB 에 `ROLE_HIERARCHY` 테이블이 존재 한다. 

    - 이 DB 에 저장된 계층 정보를 formatting 한 정보를 `RoleHierarchy` 클래스가 가지고있다.

        - `>` 로 계층 관계가 formatting 된다. 

    - 그래서 `RoleHierarchyVoter` 가 인가 정보를 심사할 때 `RoleHierarchy` 를 참조하여 심사한다.

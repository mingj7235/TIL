

Object Spec - YAML

    YAML 로 설정한다. 

    즉, 명세 를 저장하는 것. 

    - apiVersion
        - apps/v1, v1, ...

    - kind 
        - Pod, Deployment, Servce, Ingress, ReplicaSet ..

    - metadata
        - name, label, namespace,..

    - spec
        - 각종 설정 (매우 다양하다.)

    - status
        - 시스템에서 관리하는 최신상태 
        - read-only로 관리된다.



API 호출하기

    - Desired state 를 다양한 오브젝트로 정의(spec) 하고, API 서버에 yaml 형식으로 전달하는 것.

    - 명세를 전달하면 k8s 가 알아서 띄우게 된다.


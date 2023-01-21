# IAM - 정책

## 개념

- AWS 리소스에 대한 액세스 권한을 정의 한 것

- 사용자, 그룹, 역할에 정책을 연결하여 사용

- JSON 문서 형식으로 이루어짐

- 정책이 명시되지 않는 경우, 기본적으로 모든 요청이 거부됨


<br>

## 정책 JSON 구조

``` json
{
    "Statement" : [{
        "Effect" : "effect",
        "Action" : "action",
        "Resource" : "arn",
        "Condition" : {
            "condition" :{
                "key" : "value"
            }
        }
    }]
}
```

- Effect : Statement 에 대한 Access 또는 Deny

- Actions : 권한에 대한 작업 목록, 즉 무엇을 할것인지

- Resource : 권한이 적용되는 리소스, 즉 어떤 리소스에 권한을 적용시킬 것인지

- Condition : 정책이 적용되는 세부 조건 (옵션 사항)



## 정책 예시

![스크린샷 2023-01-21 오후 5 30 52](https://user-images.githubusercontent.com/74750901/213859736-35d9ff1c-243a-4791-9227-4cb98cd90064.png)

- 예시 해석

    - Lambda 서비스에 대한 정책을 명시한 것이다.

    - 기본적으로는 모든 권한을 허용 (Allow) 한다.

    - 다만, 200.100.16.0/20 IP 네트워크로부터의 '함수 생성(CreateFunction)', '함수 삭제(DeleteFunction)' 은 권한이 거부된다. 


![스크린샷 2023-01-21 오후 5 35 50](https://user-images.githubusercontent.com/74750901/213859755-0982b81a-8790-4680-856f-2ea10e1a3a5f.png)

- 예시 해석

    - EC2 서비스에 대한 정책 명시

    - 사용자 소스 IP 대역이 10.100.100.0/24 인 경우에만 EC2 인스턴스를 종료할 수 있다.

    - us-east-1 리전이 아닌 경우에는 (즉, 다른 리전에 발생하는) 모든 작업이 거부된다. 

    - 전체 정리 : 사용자 소스 IP 가 10.100.100.0/24 대역인 경우에만 us-east-1 리전의 ec2 인스턴스를 종료할 수 있다.

<br>

## 권한 경계 (Permission Boundary)

- IAM 사용자 또는 역할에 `최대 권한을 제한` 하는 기능

- IAM 사용자에게 S3, CloudWatch, Ec2 만 권리할 수 있게 하려면 아래와 같이 정책을 적용한다. 

``` json
{
    "Statement" : [
        {
            "Effect" : "Allow",
            "Action" : [
                "s3:*",
                "cloudwatch:*",
                "ec2:*"
            ],
            "Resource" : "*"
        }
    ]
}
```

- 만약, IAM 사용자에게 두개의 정책이 적용되어있다면, `그 두개의 정책의 교집합의 권한 만이 적용`된다.



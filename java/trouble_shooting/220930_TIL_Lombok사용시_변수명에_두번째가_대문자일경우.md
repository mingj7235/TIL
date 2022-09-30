# Lombok 사용시 변수명에 두번째 글자가 대문자일경우 생기는 문제


## 🌱 서론

- 외부 API 의 값을 받아오려고 만든 ResponseDto 의 객체중 ``dTime`` 이라는 변수가 제대로 매핑이 되지 않고 ``dtime`` 이라는 키값으로 매핑이 되어 ``null`` 을 반환하는 상황을 마주함


## 🤖 Trouble 의 시작

```java

@Getter
@Setter
@NoArgsConstructor
public class ResponseDto {

    ...

    @Getter
    @Setter
    @NoArgsConstructor
    public static class DeliveryDetail {

        private String crgNm;

        private String crgSt;

        private String scanNm;

        private String dTime;

        ...
    }

}


```

- 위와같이 ``ResponseDto`` 의 ``innerClass`` 중 하나를 만들었는데, 아래의 사진과 같은 현상이 생겼다.

[] 사진

[] 사진

- 상황의 설명

    - 가지고 오고싶은 데이터의 필드명은 ``dTime`` 이다.

    - 나는 그것에 맞도록 Response 그릇을 ``dTime`` 으로 했으나, 실제로 값을 호출하고 받아오는 ``Response Data`` 를 ``swagger`` 에서 확인해보니 ``dtime`` 으로 매핑이 되어 값이 ``null`` 이 되어 날아오게 되었다.

<br>

## 💡 Trouble Shooting

문제는 ``Lombok`` 이었다. 🔥

- Lombok 을 이용한 ``@Getter, @Setter`` 의 접근일 경우, 자동으로 카멜 케이스를 이용한 ``getter, setter`` 메소드를 만들 때, ``getDTime(), setDTime()`` 으로 생성하기 때문에 이러한 문제가 발생한 것이다.

[] 사진 

<i>인텔리 제이에서 Structure (단축키 : command + 7) 에서 확인한 Lombok 으로 생성된 Getter, Setter</i>

- ``getter, setter`` 메소드 명은 ``get, set`` 다음에 이어지는 단어는 반드시 대문자로 작성되기 때문이다. 

- 그런데, 여기서 문제는 ``dTime`` 변수의 두 번째 글자도 대문자이기 때문에 구분이 되지 않아서 이런 문제가 발생한다. 

- 인텔리제이가 제공하는 ``Generate Getters and Setters`` 를 활용하면 메서드명이 ``getdTime(), setdTime()`` 으로 생성이되어 접근이 가능해진다.


```java
    @NoArgsConstructor
    public static class DeliveryDetail {

        private String crgNm;

        private String crgSt;

        private String scanNm;

        private String dTime;


        // Lombok 에 의존하지않고 직접 getter, setter 생성
        public String getCrgNm() {
            return crgNm;
        }

        public void setCrgNm(String crgNm) {
            this.crgNm = crgNm;
        }

        public String getCrgSt() {
            return crgSt;
        }

        public void setCrgSt(String crgSt) {
            this.crgSt = crgSt;
        }

        public String getScanNm() {
            return scanNm;
        }

        public void setScanNm(String scanNm) {
            this.scanNm = scanNm;
        }

        public String getdTime() {
            return dTime;
        }

        public void setdTime(String dTime) {
            this.dTime = dTime;
        }
    }
```

## 🙋🏻‍♂️ 결론

> 첫 번째 단어가 한 글자로 시작하며, 소문자이고 카멜케이스로 변수를 사용한다면 주의해서 Getter, Setter 를 생성해야한다.

# Lombok 사용시 변수명에 두번째 글자가 대문자일경우 생기는 문제

```java
    @Getter
    @Setter
    @NoArgsConstructor
    public static class DeliveryDetail {

        private String crgNm;

        private String crgSt;

        private String scanNm;

        private String dTime;
    }

```


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

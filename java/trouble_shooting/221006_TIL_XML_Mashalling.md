# XML Mashalling

<br>

## 공공 API 에서 내려주는 XML 데이터

```xml
<response>
    <header>
    ...
    </header>
    <body>
        <items>
            <item>
                <dateKind>01</dateKind>
                <dateName>추석</dateName>
                <isHoliday>Y</isHoliday>
                <locdate>20150926</locdate>
                <seq>1</seq>
            </item>
                <item>
                <dateKind>01</dateKind>
                <dateName>추석</dateName>
                <isHoliday>Y</isHoliday>
                <locdate>20150927</locdate>
                <seq>1</seq>
            </item>
                ...
        </items>
        <numOfRows>10</numOfRows>
        <pageNo>1</pageNo>
        <totalCount>18</totalCount>
    </body>
</response>

```
<br>

## Feign Client

```java
    @GetMapping (value = "${feign.data.api}", produces = MediaType.APPLICATION_XML_VALUE)
    DATAResponse getHolidayInfo (@RequestParam ("serviceKey") String serviceKey,
                                 @RequestParam (value = "solYear", required = false) String year,
                                 @RequestParam (value = "solMonth", required = false) String month);

}

}
```

<br>

## Feign Service

```java
@Slf4j
@Service
@Transactional
@RequiredArgsConstructor
public class DATAFeignService {

    private final DATAFeignClient dataFeignClient;

    @Value("${feign.data.key}")
    public String serviceKey;

    public DATAResponse getHolidayInfo (final @NotBlank String year,
                                        final @NotBlank String month) {

        return dataFeignClient.getHolidayInfo(serviceKey, year, month);
    }
}
```

<br>

## DATAResponse (XML 을 받을 JAVA 객체)

```java
@Getter
@Setter
@ToString
@XmlAccessorType (XmlAccessType.FIELD)
@XmlRootElement (name = "response")
public class DATAResponse {

    @XmlElement (name = "body")
    private DATABody dataBody;

    @Getter
    @Setter
    @ToString
    @XmlAccessorType (XmlAccessType.FIELD)
    public static class DATABody {

        @XmlElement (name = "items")
        private items items;
    }

    @Getter
    @Setter
    @ToString
    @XmlRootElement (name = "items")
    @XmlAccessorType (XmlAccessType.FIELD)
    public static class items {
        @XmlElement (name = "item")
        public List<Item> item;
    }

    @Getter
    @Setter
    @ToString
    @XmlRootElement (name = "item")
    @XmlAccessorType (XmlAccessType.FIELD)
    public static class Item {

        @XmlElement (name = "dateKind")
        private String dateKind;

        @XmlElement (name = "dateName")
        private String dateName;

        @XmlElement (name = "idHoliday")
        private String idHoliday;

        @XmlElement (name = "locdate")
        private String locdate;

        @XmlElement (name = "seq")
        private String seq;
    }
}
```

- 받을 필요 없는 XML 정보는 삭제하여 Java 객체 생성

<br>

## 성공적으로 받은 객체 (Json 형태)

![스크린샷 2022-10-06 오후 12 21 55](https://user-images.githubusercontent.com/74750901/194207202-4db31a22-abac-4cfa-85de-dae47b5a9765.png)


# Feign Client Til

## Configuration 을 등록한 Feign Client 의 name 을 가져오는 방법

```java
@Bean
    public RequestInterceptor requestInterceptor () {
        return requestTemplate -> {

            if (requestTemplate.feignTarget().name().equals("snapsFeign")){
                requestTemplate.header("x-webling-brand", "SNAPS");
                requestTemplate.header("x-webling-token", tokenVerify.generateToken(BrandCode.SNAPS, Instant.now().getEpochSecond(), ApiConstants.API_TYPE_FEIGN_ORDER));
            }

            if (requestTemplate.feignTarget().name().equals("opmFeign")) {
                requestTemplate.header("x-webling-brand", "OHPRINT");
                requestTemplate.header("x-webling-token", tokenVerify.generateToken(BrandCode.OHPRINTME, Instant.now().getEpochSecond(), ApiConstants.API_TYPE_FEIGN_ORDER));
            }

        };
    }
```

- requestTemplate.feignTarget().name() 으로 불러올 수 있음

<br>

## FeignClient

```java
@FeignClient(name = "${feign.snaps.name}", url = "${feign.snaps.url}", configuration = {FeignHeaderConfiguration.class})
public interface SnapsFeignClient {

    public void feignMethod (final @NotNull Request request) {
        ...

        ....
    }
} 

```

<br>

## FeignClient 호출 시점에 Header 정보를 넣고 싶은 경우

### Feign

```java
    @PostMapping (value = "url/url")
    Response method (@RequestHeader("cookie") String headers, final @NotBlank String param);
```

### Service (Feign 주입)

```java
feignClient.method(headers, param);
```
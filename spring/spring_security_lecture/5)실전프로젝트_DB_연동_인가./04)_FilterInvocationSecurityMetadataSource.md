# FilterInvocationSecurityMetadataSource

## 개요

![스크린샷 2022-10-08 오전 10 44 00](https://user-images.githubusercontent.com/74750901/194685457-6b75c6d2-5da0-4e13-a3d3-a4733bc4b9d7.png)


- 사용자가 접근하고자 하는 Url 자원에 대한 권한 정보 추출

    - 요청자원에 대해 필요한 권한 정보를 추출하여 전달하는것

- AccessDecisionManager 에게 전달하여 인가 처리 수행

- DB로 부터 자원 및 권한 정보를 매핑하여 Map으로 관리 

    - 접근하려는 자원 (Resources 테이블), 권한 정보 (Role 테이블) 

    - DB 테이블에서 매핑

- 사용자의 매 요청마다 요청정보에 매핑된 권한 정보 확인

    - Map 에 저장되어있는 자원과 권한 정보를 확인

    - Filter 에게 전달 -> AccessDecisionManager 에게 전달

<br>

## 흐름도

![스크린샷 2022-10-08 오전 10 47 42](https://user-images.githubusercontent.com/74750901/194685461-fb734b43-ef0f-4786-81d7-9adcab40bd4e.png)


- FilterSecurityInterceptor

    - 권한을 처리하는 가장 마지막에 위치하는 보안 필터


## 구현

### UrlFilterInvocationSecurityMetadataSource

<br>

```java
/**
     * LinkedHashMap
     *
     * key : RequestMatcher , 즉 URL 정보
     * value : List<ConfigAttribute>>, 즉 권한들의 리스트
     *
     */
    private LinkedHashMap<RequestMatcher, List<ConfigAttribute>> requestMap = new LinkedHashMap<>();


    @Override
    public Collection<ConfigAttribute> getAttributes(final Object object) throws IllegalArgumentException {

        HttpServletRequest request = ((FilterInvocation) object).getRequest();

        requestMap.put(new AntPathRequestMatcher("/mypage"), List.of(new SecurityConfig("ROLE_USER")));

        if (requestMap != null) {
            for (Map.Entry<RequestMatcher, List<ConfigAttribute>> entry : requestMap.entrySet()) {
                RequestMatcher matcher = entry.getKey(); // URL 정보
                if (matcher.matches(request)) {
                    return entry.getValue(); // URL 정보에 매칭되는 권한 정보 (List)
                }
            }
        }

        return null;
    }
```

### SecurityConfig

<br>

```java

...

    @Bean
    public FilterSecurityInterceptor customFilterSecurityInterceptor() throws Exception {
        FilterSecurityInterceptor filterSecurityInterceptor = new FilterSecurityInterceptor();
        filterSecurityInterceptor.setSecurityMetadataSource(urlFilterInvocationSecurityMetadataSource());
        filterSecurityInterceptor.setAccessDecisionManager(affirmativeBased());
        filterSecurityInterceptor.setAuthenticationManager(authenticationManagerBean());
        return filterSecurityInterceptor;
    }

    private AccessDecisionManager affirmativeBased() { // 3가지 DecisionManager 중에 가장 무난한 녀석
        return new AffirmativeBased(getAccessDecisionVoters());
    }

    private List<AccessDecisionVoter<?>> getAccessDecisionVoters() {
        return List.of(new RoleVoter());
    }

    @Bean
    public FilterInvocationSecurityMetadataSource urlFilterInvocationSecurityMetadataSource() {
        return new UrlFilterInvocationSecurityMetadataSource();
    }

...
```

     
FilterSecurityInterceptor 를 커스텀 하여 Bean 등록

- FilterSecurityInterceptor 는 권한을 체크하는 마지막에 위치하는 Filter

- 인가처리를 내가 만든 UrlFilterInvocationSecurityMetadataSource 로 하도록 설정하는 것임.

- 이 필터를 커스텀하기 위해서는 3가지를 set 해줘야한다.

    - setSecurityMetadataSource
    - setAccessDecisionManager
    - setAuthenticationManager
     

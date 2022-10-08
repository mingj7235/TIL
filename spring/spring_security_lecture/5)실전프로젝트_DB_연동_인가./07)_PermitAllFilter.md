# PermitAllFilter

## 구현 방식

![스크린샷 2022-10-08 오후 1 39 06](https://user-images.githubusercontent.com/74750901/194689808-4f45dbf5-b61d-4e08-9078-95dc1350f478.png)


### 내부 동작 원리

- 인증 및 권한 심사를 할 필요가 없는 자원 들을 미리 설정하여 바로 리소스 접근이 가능하게 하는 Filter

- ``List<ConfigAttribute>`` 가 ``null`` 이면 즉, 해당 ``request`` 의 자원에 매핑된 권한 정보가 없으면 권한 심사없이 통과를 시킨다. 

    - `null` 아니면, 즉, 해당 자원에 매핑된 권한 정보가 있다면 인가 권한을 심사한다. 

<br>

### 응용 동작 구현

- 목표 : 자원의 낭비를 줄이는 것. 굳이 심사하지 않아도 되는 자원을 미리 저장하여 앞에서 잘라버리는 것.

- FilterSecurityInterceptor 를 상속한 PermitAllFilter 구현

- FilterSecurityInterceptor 와 AbstractSecurityInterceptor 에 가는 과정에 중간 로직을 구현을 하여 그 뒤로 가지 않고도 미리 처리를 시키도록 하는 것. 

- 즉, null 인경우에 인증을 맡기는 것이 아니라, 접근을 하였을 때 인증이 필요하지 않은 자원을 미리 정해놓아서, 그 후까지 갈 필요없이 앞에서 미리 능동적으로 제한을 막을 수 있도록 하는 것.

```java
public class PermitAllFilter extends FilterSecurityInterceptor {

    private static final String FILTER_APPLIED = "__spring_security_filterSecurityInterceptor_filterApplied";
    private boolean observeOncePerRequest = true;

    private List<RequestMatcher> permitAllRequestMatchers = new ArrayList<>();

    // 생성자로 받아온다. (SecurityConfig에서 설정)
    public PermitAllFilter (String... permitAllResources) {
        for (String permitAllResource : permitAllResources) {
            permitAllRequestMatchers.add(new AntPathRequestMatcher(permitAllResource));
        }
    }

    @Override
    protected InterceptorStatusToken beforeInvocation(final Object object) {

        boolean permitAll = false;
        HttpServletRequest request = ((FilterInvocation) object).getRequest();

        for (RequestMatcher requestMatcher : permitAllRequestMatchers) {
            if(requestMatcher.matches(request)) {
                permitAll = true;
                break;
            }
        }

        // 권한 수행하지 않는다는 의미
        if (permitAll) {
            return null;
        }

        return super.beforeInvocation(object);
    }

    public void invoke(FilterInvocation filterInvocation) throws IOException, ServletException {
        if (isApplied(filterInvocation) && this.observeOncePerRequest) {
            filterInvocation.getChain().doFilter(filterInvocation.getRequest(), filterInvocation.getResponse());
            return;
        }
        if (filterInvocation.getRequest() != null && this.observeOncePerRequest) {
            filterInvocation.getRequest().setAttribute(FILTER_APPLIED, Boolean.TRUE);
        }

        // 부모의 beforInvocation 메소드를 호출하지 않고,
        // 여기서 override 한 메소드를 호출하여 비용을 아끼는 것임.
        InterceptorStatusToken token = beforeInvocation(filterInvocation);

        try {
            filterInvocation.getChain().doFilter(filterInvocation.getRequest(), filterInvocation.getResponse());
        }
        finally {
            super.finallyInvocation(token);
        }
        super.afterInvocation(token, null);
    }

    private boolean isApplied(FilterInvocation filterInvocation) {
        return (filterInvocation.getRequest() != null)
                && (filterInvocation.getRequest().getAttribute(FILTER_APPLIED) != null);
    }


}

```

- invoke() 는 부모클래스인 `FilterSecurityInterceptor` 의 메소드를 그대로 가져와서 내부의 `InterceptorStatusToken token = beforeInvocation(filterInvocation);` 만 변경한 것.

    - `InterceptorStatusToken token = super.beforeInvocation(filterInvocation);` 에서 `super` 를 빼고, 내부에서 다시 재정의한 `berforeInvocation ` 메소드를 호출하도록 변경함

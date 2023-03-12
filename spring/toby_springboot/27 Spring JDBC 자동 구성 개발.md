
## 🌱 Step 1

````ad-note
title: JDBC 자동 구성 개발

<br>

```ad-info
title: MyDataSourceProperties

~~~java

@Getter  
@Setter  
@MyConfigurationProperties(prefix = "data")  
public class MyDataSourceProperties {  
  
    private String driverClassName;  
    private String url;  
    private String username;  
    private String password;  
}

~~~

- Property 정보들이 매핑될 클래스. 
- Property 정보들을 클래스 분리하는 것임 [[26 Property 클래스 분리]] 참고
 
```

<br>
```ad-info
title: DataSourceConfig

~~~java


~~~
```

````
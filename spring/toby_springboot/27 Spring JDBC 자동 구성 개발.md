
## 🌱 Step 1

````ad-note
title: JDBC 자동 구성 개발

<br>

```ad-info
title: MyDataSourceProperties

~~~java

@Getter  
@Setter  
@MyConfigurationProperties(prefix = "data") // 1
public class MyDataSourceProperties {  
  
    private String driverClassName;  
    private String url;  
    private String username;  
    private String password;  
}

~~~

- Property 정보들이 매핑될 클래스. 
- Property 정보들을 클래스 분리하는 것임 [[26 Property 클래스 분리]] 참고

- 1 : prefix 설정은 application.properties 에서 prefix 로 붙을 값을 의미한다.
```

<br>

```ad-info
title: DataSourceConfig

~~~java
@MyAutoConfiguration  
@ConditionalMyOnClass("org.springframework.jdbc.core.JdbcOperations") // 1
@EnableMyConfigurationProperties(MyDataSourceProperties.class) // 2
public class DataSourceConfig {  
  
    @Bean  
    @ConditionalMyOnClass("com.zaxxer.hikari.HikariDataSource") // 3
    @ConditionalOnMissingBean 
    DataSource hikariDataSource(MyDataSourceProperties properties) {  
  
        HikariDataSource dataSource = new HikariDataSource();  
  
        dataSource.setDriverClassName(properties.getDriverClassName());  
        dataSource.setJdbcUrl(properties.getUrl());  
        dataSource.setUsername(properties.getUsername());  
        dataSource.setPassword(properties.getPassword());  
  
        return dataSource;  
  
    }  
    @Bean  
    @ConditionalOnMissingBean    
    DataSource dataSource(MyDataSourceProperties properties) throws ClassNotFoundException {  
        SimpleDriverDataSource dataSource = new SimpleDriverDataSource();  
  
        dataSource.setDriverClass((Class<? extends Driver>) Class.forName(properties.getDriverClassName()));  
        dataSource.setUrl(properties.getUrl());  
        dataSource.setUsername(properties.getUsername());  
        dataSource.setPassword(properties.getPassword());  
  
        return dataSource;  
    }  
}

~~~

- 1 : JdbcOperations 클래스를 의존성으로 가지고 있을 경우 해당 Configuration 이 동작하도록 한다. 
	- JdbcOperations : 'org.springframework:spring-jdbc' 패키지를 dependency 에 추가해야한다.

- 2 : MyDataSourceProperties 클래스에 property 가 매핑 되는 것을 의미한다. 

- 3 : HikariDataSource 를 의존성으로 가지고 있을 경우 해당 Bean 이 등록되도록 한다. 
```

<br>

```ad-info
title: TEST 코드 작성

~~~java

@ExtendWith(SpringExtension.class)  
@ContextConfiguration(classes = Application.class) // 1   
@TestPropertySource("classpath:/application.properties") // 2 
public class DataSourceTest {  
  
    @Autowired  
    DataSource dataSource;  
  
    @Test  
    void connect () throws SQLException {  
        Connection connection = dataSource.getConnection();  
        connection.close();  
    }  
}
~~~

- 1 : Bean 정보를 끌어오는 시작점이 되는 클래스를 지정. 이렇게 Bean 을 가지고와서 테스트에 사용할 수 있도록 함

- 2 : Test 를 실행하는 동안 application.properties 의 구성정보를 가지고 올 수 있도록 설정을 함  

```

````


## 📚 Step 2


# Custom Dialect Configuration

## 개요

- DB 방언을 Custom 하여 QueryDsl 에서 MySQL 에서 제공하는 함수를 사용하게 끔 하기 위함

## application.yml 설정

```yaml
spring:
  jpa:
    open-in-view: false
    hibernate:
      ddl-auto: update
      use-new-id-generator-mappings: false
    show-sql: true

    # Custom 한 DB 방언의 경로
    database-platform: com.snaps.production.framework.config.MysqlCustomDialect

    properties:
      hibernate.format_sql: true
```

<br>

## MysqlCustomDialect

```java
public class MysqlCustomDialect extends MySQL8Dialect {

    public MysqlCustomDialect() {
        super();
        this.registerFunction("GROUP_CONCAT", new StandardSQLFunction("group_concat", StandardBasicTypes.STRING));
//        this.registerFunction("GROUP_CONCAT", new SQLFunctionTemplate(StandardBasicTypes.STRING, "group_concat(?1)"));
        this.registerFunction("FN_GET_OPTION_CODE", new StandardSQLFunction("fn_get_option_code", StandardBasicTypes.STRING));
        this.registerFunction("fn_get_option_spc_code", new StandardSQLFunction("fn_get_option_spc_code", StandardBasicTypes.STRING));
        this.registerFunction("CONCAT_WS", new StandardSQLFunction("concat_ws", StandardBasicTypes.STRING));
        this.registerFunction("FN_GET_COMMON_CODE_LANG", new StandardSQLFunction("fn_get_common_code_lang", StandardBasicTypes.STRING));
        this.registerFunction("FN_GET_CJ_DLVR_NUMBER_56", new StandardSQLFunction("fn_get_cj_dlvr_number_56", StandardBasicTypes.STRING));
        this.registerFunction("FN_GET_WORK_GROUP_STATUS", new StandardSQLFunction("fn_get_work_group_status", StandardBasicTypes.STRING));
    }
}

```

<br>

## QueryDSL 에서 설정한 Function 활용

```java

    ...

        Expressions.stringTemplate("group_concat({0})",
                new CaseBuilder()
                        .when(productOption.workPrintSort.eq("1"))
                        .then(options.workPrintName)
                        .otherwise(Expressions.nullExpression())

    ...

```
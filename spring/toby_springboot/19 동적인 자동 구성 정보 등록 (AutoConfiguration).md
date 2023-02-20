
## ImportSelector

ImportSelect 를 구현한 클래스를 사용하면, Configuration 클래스들을 프로그램에서 동적으로 가져올 수 있다. 
즉, 외부설정 파일이나, DB 의 정보를 통해 동적으로 Configuration 클래스를 가져올 수 있다는 것


## 스텝 1 

ImportSelect 인터페이스를 확장한 DeferredImportSelector 를 구현한 클래스를 만들고, selectImports() 메소드를 구현한다.

```java
public class MyAutoConfigurationImportSelector implements DeferredImportSelector {  
  
    @Override  
    public String[] selectImports(final AnnotationMetadata importingClassMetadata) {  
        return new String[] {  
                "com.joshua.config.autoconfig.DispatcherServletConfig",  
                "com.joshua.config.autoconfig.TomcatWebServerConfiguration"  
        };  
    }  
}
```

@Import 를 메타 어노테이션으로 가지고 있는 @EnableMyAutoConfiguration 에서 @Import 에 위에서 구현한 Selector 클래스를 넣어준다.


```java
@Retention(RetentionPolicy.RUNTIME)  
@Target(ElementType.TYPE)  
@Import(MyAutoConfigurationImportSelector.class)  
public @interface EnableMyAutoConfiguration {  
  
}
```

- 지금은 selectImports 메소드가 리턴하는 외부 설정 정보 Bean 이 하드코딩 되어있으나, 다음 단계에서 동적으로 파일을 분리할 것

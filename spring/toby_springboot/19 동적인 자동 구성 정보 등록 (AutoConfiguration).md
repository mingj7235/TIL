
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


## 스탭 2

- MyAutoConfiguration.class 생성
```java
@Retention(RetentionPolicy.RUNTIME)  
@Target(ElementType.TYPE)  
@Configuration  
public @interface MyAutoConfiguration {  
  
}
```
- Selector 에서 구현한 load 메소드에서 구성정보 후보를 가져올 어노테이션


```java
public class MyAutoConfigurationImportSelector implements DeferredImportSelector {  
  
    private final ClassLoader classLoader;  
  
    public MyAutoConfigurationImportSelector(final ClassLoader classLoader) {  
        this.classLoader = classLoader;  
    }  
  
    @Override  
    public String[] selectImports(final AnnotationMetadata importingClassMetadata) {  
        List<String> autoConfigs = new ArrayList<>();  
  
        ImportCandidates.load(MyAutoConfiguration.class, classLoader).forEach(autoConfigs::add);  
  
        return autoConfigs.toArray(String[]::new);  
    }  
}

```

- ImportCandidats.load() 메소드는 자동 구성 정보의 후보를 로드하는 메소드인데, 설명은 이러하다
```
/**  
 * Loads the names of import candidates from the classpath. * * The names of the import candidates are stored in files named * {@code META-INF/spring/full-qualified-annotation-name.imports} on the classpath.  
 * Every line contains the full qualified name of the candidate class. Comments are * supported using the # character. * @param annotation annotation to load  
 * @param classLoader class loader to use for loading  
 * @return list of names of annotated classes  
 */
```

즉, ImportCandidats.load()는, META-INF/spring 디렉토리 내부에, {매개변수로 들어간 어노테이션 클래스의 풀네임.imports} 파일에 작성되어 있는 classpath 정보를 가져오고,
그 ClassPath 의 클래스들은 자동 구성 정보의 후보들이 된다.

- META-INF/spring 안에 있는 com.joshua.config.MyAutoConfiguration.imports 파일
```java
com.joshua.config.autoconfig.TomcatWebServerConfiguration  
com.joshua.config.autoconfig.DispatcherServletConfig
```

- 구성 파일 후보들의 classpath 를 넣었다.  
---
title: SpringBoot에 Multi Data Source 적용하기
date: 2017-07-04
categories:
  - Develop
  - Spring
tags:
  - Spring Boot
  - Spring Data JPA
---

### DataSource 설정
```yml
// application.yml
// DB를 나누기 위해, 메모리 디비를 두개 띄운다

app.datasource.blue:
  url: jdbc:h2:mem:blue
  username: sa
  password:
 
app.datasource.green:
  url: jdbc:h2:mem:green
  username: sa
  password:
```

### Package Tree
![](/images/jpa/folder-tree.png)

### DataSourceConfig
```java
@Configuration
@EnableJpaRepositories(
        basePackages = {BLUE_ENTITY_BASE_PACKAGES},
        entityManagerFactoryRef = "blueEntityManagerFactory",
        transactionManagerRef = "blueTransactionManager")
public class BlueDataBaseConfig {

    public static final String BLUE_ENTITY_BASE_PACKAGES = "com.example.demo.blue";
    // 예제엔 Entity와 Repository가 같은 패키지에 있으므로, 위에 패키지 하나로 공유한다

    @Bean
    @Primary
    @ConfigurationProperties("app.datasource.blue")
    public DataSourceProperties blueDataSourceProperties() {
        return new DataSourceProperties();
    }

    @Bean
    @Primary
    public DataSource blueDataSource() {
        return blueDataSourceProperties().initializeDataSourceBuilder().build();
    }

    @Bean
    @Primary
    public LocalContainerEntityManagerFactoryBean blueEntityManagerFactory(EntityManagerFactoryBuilder builder) {
        return builder
                .dataSource(blueDataSource())
                .packages(BLUE_ENTITY_BASE_PACKAGES)
                .persistenceUnit("blue")
                .build();
    }

    @Bean
    @Primary
    public PlatformTransactionManager blueTransactionManager(@Qualifier("blueEntityManagerFactory") EntityManagerFactory entityManagerFactory) {
        return new JpaTransactionManager(entityManagerFactory);
    }
}

```
 - Blue, Green Config가 크게 다르지 않아서 생략.. 하단에 Github을 참고할것(Green에는 `@Primary`가 빠져있고, 이름만 green으로 시작함)
 - 단일 DataSouce에서 JPA의 `EntityManagerFactory`, `TransactionManager`가 존재하므로 이를 나눠줘야 함
 - TransactionManager가 나뉘어졌으므로, `@Transcation`을 사용할때 주의할것
 - SpringBoot 1.3.X 버전부터 `EntityManagerFactoryBuilder`의 패키지가 변경되었다 (org.springframework.boot.autoconfigure.orm.jpa.EntityManagerFactoryBuilder -> org.springframework.boot.orm.jpa.EntityManagerFactoryBuilder)

### 주의사항
> 반드시 1개의 `DataSource`에 `@Primary`키워드를 붙여야 한다.

### 샘플
 - [Github 예제코드](https://github.com/choduk/springboot-two-datasource)
 - 
 
## 참고자료
 - [Spring.io 공식문서](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto-two-datasources)

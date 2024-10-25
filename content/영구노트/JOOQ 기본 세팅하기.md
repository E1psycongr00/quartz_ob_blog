---
tags:
  - 완성
  - JAVA
  - SQL
  - JOOQ
aliases: 
date: 2024-08-26
title: JOOQ 기본 세팅하기
---
작성 날짜: 2024-08-26
작성 시간: 21:02


----
## 내용(Content)

### JOOQ 셋팅하기

#### 의존성 등록하기

Spring Boot에서 JOOQ 의존성을 쉽게 등록할 수 있다.

```kotlin
dependencies {
	implementation("org.springframework.boot:spring-boot-starter-jooq")
	implementation("org.springframework.boot:spring-boot-starter-web")
	compileOnly("org.projectlombok:lombok")
	runtimeOnly("com.h2database:h2")
	annotationProcessor("org.projectlombok:lombok")
	testImplementation("org.springframework.boot:spring-boot-starter-test")
	testRuntimeOnly("org.junit.platform:junit-platform-launcher")
}
```

JOOQ의 경우 spring-boot-starter-jooq 를 지원한다. 그렇기 떄문에 쉽게 의존성 등록이 가능하다.

#### 빈 등록하기

JOOQ Bean을 등록한다.

```java
import org.jooq.SQLDialect;
import org.jooq.impl.DataSourceConnectionProvider;
import org.jooq.impl.DefaultConfiguration;
import org.jooq.impl.DefaultDSLContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.datasource.TransactionAwareDataSourceProxy;

import javax.sql.DataSource;

@Configuration
public class JooqConfig {


    @Bean
    public DataSourceConnectionProvider connectionProvider(DataSource dataSource) {
        return new DataSourceConnectionProvider(new TransactionAwareDataSourceProxy(dataSource));
    }

    @Bean
    public DefaultDSLContext dsl(org.jooq.Configuration configuration) {
        return new DefaultDSLContext(configuration);
    }

    @Bean
    public DefaultConfiguration configuration(DataSourceConnectionProvider connectionProvider) {
        DefaultConfiguration jooqConfiguration = new DefaultConfiguration();
        jooqConfiguration.set(connectionProvider);
        jooqConfiguration.set(SQLDialect.H2);
        return jooqConfiguration;
    }
}
```

이 부분을 조금 더 자세히 분석해보자

##### DataSourceConnectionProvider

```java
    @Bean
    public DataSourceConnectionProvider connectionProvider(DataSource dataSource) {
        return new DataSourceConnectionProvider(new TransactionAwareDataSourceProxy(dataSource));
    }
```

이 Bean은 JOOQ가 DB와 연결을 관리하는 데 사용된다.

- `TransactionAwareDataSourceProxy` 를 사용하여 Spring의 트랜잭션 관리와 통합된다.
- JOOQ 공식 문서에 따르면, spring과 함께 사용할 때 트랜잭션 관리를 위해 추천되는 방식이다.

##### DefaultDSLContext

```java
   @Bean
   public DefaultDSLContext dsl(org.jooq.Configuration configuration) {
       return new DefaultDSLContext(configuration);
   }
```

- DslContext는 JOOQ 쿼리 빌더를 위해 사용하는 객체이다.
- 이 Bean을 통해 어플리케이션 전체에서 JOOQ의 DSL를 사용할 수 있게 된다.

>[!info]
>JOOQ의 DSL(Domain Specific Language)에 대해서 설명하겠다.
>- DSL의 정의
>	- JOOQ의 DSL은 SQL을 Java 코드로 표현하기 위한 특별한 API다.
>	- "Domain Specific Language"의 약자로, 특정 도메인에 특화된 프로그래밍 언어를 의미한다.
>- DSL의 목적
>	- SQL 쿼리를 타입 안전하고, IDE 친화적인 방식으로 작성할 수 있게 함
>	- DB 스키마와 일치하는 Java 코드를 생성하여 컴파일 시점에 오류를 잡을 수 있게 한다.

##### DefaultConfiguration

```java
   @Bean
   public DefaultConfiguration configuration(DataSourceConnectionProvider connectionProvider) {
       DefaultConfiguration jooqConfiguration = new DefaultConfiguration();
       jooqConfiguration.set(connectionProvider);
       jooqConfiguration.set(SQLDialect.H2);
       return jooqConfiguration;
   }
```

- 해당 Bean은 전반적인 설정을 담당한다.
- 생성해두었던 connectionProvider를 JooQConfiguration에 주입하고, driver 또한 셋팅해준다.
- 공식 문서에 따르면 해당 Configuration을 이용해 여러 종류의 데이터베이스를 설정해서 사용할 수 있다고 한다.

#### Bean을 직접 등록하지 않고 설정하기

application.yml에 다음과 같은 내용을 추가해주면 된다.

```yaml
spring:
  datasource:
    url: jdbc:h2:mem:testdb
    driver-class-name: org.h2.Driver
    username: sa
    password:

  # my custom jooq config
  jooq:
    sql-dialect: H2
```


spring.jooq.sql-dialect=H2로 설정해두면 알아서 Spring 어플리케이션에서 Jooq에 필요한 설정을 자동으로 등록하게 된다. 그러면 이전에 설정할 때 추가한 Bean 중에 `DataSourceConnectionDriver`와 `DefaultConfiguration`을 추가할 필요가 없어진다.

그래도 해당 객체는 Bean으로 꼭 등록해줘야 한다.

```java
   @Bean
   public DefaultDSLContext dsl(org.jooq.Configuration configuration) {
       return new DefaultDSLContext(configuration);
   }
```


## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트

- [[JOOQ 소개]]

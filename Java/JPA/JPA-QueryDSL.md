## QueryDSL
----
```
  - SQL, JPQL을 코드로 작성할 수 있도록 도와주는 빌더 API
```
  + 특징
    + 컴파일 시점에 문법 오류를 찾을 수 있음
    + 동적쿼리 작성 편리함
    + 오픈소스


### JPQL vs QueryDSL
-----
```
  공통점 : 동적 쿼리 작성
```

  + JPQL
    + SQL, JPQL은 문자열이다. Type-check가 불가능하다.
    + 해당 로직 실행 전까지 작동여부 확인을 할 수 없다.
    + 해당 쿼리 실행 시점에 오류를 발견한다.
  
  + QueryDSL
    + Type-Safe (컴파일 시점에 알 수 있는) 쿼리를 날리기 위해서 사용한다.
    + IDE의 도움을 받아, 컴파일 시점에 오류를 발견할 수 있다.
    + Querydsl 동작 하는 과정은 JPQL 을 거쳐서 SQL 로 변환되서 실행한다.


###  QueryDSL 의존성
---
```java
    plugins {
        id 'org.springframework.boot' version '2.5.2'
        id 'io.spring.dependency-management' version '1.0.11.RELEASE'
        // querydsl
        id "com.ewerk.gradle.plugins.querydsl" version "1.0.10"

        id 'java'
    }

    group = 'com'
    version = '0.0.1-SNAPSHOT'
    sourceCompatibility = '1.8'

    configurations {
        compileOnly {
            extendsFrom annotationProcessor
        }
    }

    repositories {
        mavenCentral()
    }

    dependencies {
        implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
        implementation 'org.springframework.boot:spring-boot-starter-web'
        compileOnly 'org.projectlombok:lombok'
        developmentOnly 'org.springframework.boot:spring-boot-devtools'
        runtimeOnly 'com.h2database:h2'
        annotationProcessor('org.projectlombok:lombok')
        testAnnotationProcessor('org.projectlombok:lombok')
        testImplementation 'org.springframework.boot:spring-boot-starter-test'

        //querydsl 추가
        implementation 'com.querydsl:querydsl-jpa'
        implementation 'com.querydsl:querydsl-apt'

    }

    test {
        useJUnitPlatform()
    }


    //querydsl 추가 시작
    def querydslDir = "$buildDir/generated/querydsl"
    querydsl {
        jpa = true
        querydslSourcesDir = querydslDir
    }
    sourceSets {
        main.java.srcDir querydslDir
    }
    configurations {
        querydsl.extendsFrom compileClasspath
    }
    compileQuerydsl {
        options.annotationProcessorPath = configurations.querydsl
    }
    //querydsl 추가 끝


  - querydsl-apt: Querydsl 관련 코드(ex: QHello) 생성 기능 제공
  - querydsl-jpa: Querydsl JPA 관련 기능 제공
```

### 세팅
-----
  + jpaQueryFactory 빈 설정
  ```java
  @EnableJpaAuditing
  @Configuration
  public class QuerydslConfig {

      @PersistenceContext
      private EntityManager entityManager;

      @Bean
      public JPAQueryFactory jpaQueryFactory() {
          return new JPAQueryFactory(entityManager);
      }
  }
  ```
  
  + Repository 세팅
  + extends / implements 사용하지 않기
    + JpaQueryFactory만 의존 받으면 깔끔하게 사용 가능
  ```java

  // 기존 방법 1. 
  public interface CustomerRepository extends JpaRepository<Customer, Long>, CustomerQueryRepository {} 


  // 기존 방법2
  public class CustomerQueryRepository extends QuerydslRepositorySupport {}

  // 추천하는 방법
  @Repository
  @RequiredArgsConstructor 
  public class CustomerQueryRepository {
      private final JpaQueryFactory queryFactory; // 물론 이를 위해서는 빈으로 등록을 해줘야 한다. 
  }
  ```




### 기본 문법
-----
  + Q클래스 인스턴스를 사용하는 2가지 방법
```java
  QCustomer qCustomer = new QCustomer("c"); //별칭 직접 지정
  QCustomer qCustomer = QCustomer.customer; //기본 인스턴스 사용
```

  + 조건문 검색
  ```java
  
      public Customer findByLastName(String lastName){
        return queryFactory.selectFrom(customer)
                           .where(customer.lastName.eq(lastName))
                           .fetchOne();
    }
  ```


참고 사이트 : https://github.com/Youngerjesus/Querydsl

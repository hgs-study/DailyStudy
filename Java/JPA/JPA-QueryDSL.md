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
  + 일반 용법
    + from: 쿼리 소스를 추가한다. 첫 번째 인자는 메인 소스가 되고, 나머지는 변수로 취급한다.
    + where: 쿼리 필터를 추가한다. 가변인자나 and/or 메서드를 이용해서 필터를 추가한다.
    + groupBy: 가변인자 형식의 인자를 기준으로 그룹을 추가한다.
    + having: Predicate 표현식을 이용해서 "group by" 그룹핑의 필터를 추가한다.
    + orderBy: 정렬 표현식을 이용해서 정렬 순서를 지정한다. 숫자나 문자열에 대해서는 asc()나 desc()를 사용하고, OrderSpecifier에 접근하기 위해 다른 비교 표현식을 사용한다.
    + limit, offset, restrict: 결과의 페이징을 설정한다. limit은 최대 결과 개수, offset은 결과의 시작 행, restrict는 limit과 offset을 함께 정의한다.

  
  + Q클래스 인스턴스를 사용하는 2가지 방법
  ```java
    QCustomer qCustomer = new QCustomer("c"); //별칭 직접 지정
    QCustomer qCustomer = QCustomer.customer; //기본 인스턴스 사용
  ```
  
  + 테스트 데이터 생성
  ```java
    @BeforeEach
    public void init() {
        jpaQueryFactory = new JPAQueryFactory(em);

        Customer saveCustomer1 = new Customer("gs","hyun");
        Customer saveCustomer2 = new Customer("aa","kim");
        Customer saveCustomer3 = new Customer("bb","park");
        em.persist(saveCustomer1);
        em.persist(saveCustomer2);
        em.persist(saveCustomer3);
    }
  ```

  + 조건문 검색 (where(), and())
  ```java
    @DisplayName("FirstName, LastName 찾기 - and()")
    @Test
    void findByFirstNameAndLastName(){

        Customer findCustomer = jpaQueryFactory
                                    .selectFrom(customer)
                                    .where(customer.lastName.eq("hyun")
                                    .and(customer.firstName.eq("gs")))
                                    .fetchOne();

        assertEquals("gs",findCustomer.getFirstName());
    }
  ```
  + 결과

  <br/>
  
  ![image](https://user-images.githubusercontent.com/76584547/125473072-c869c74b-41ee-44bc-8007-4e2449c34b72.png)
  
  
  
  + 조건문 검색 (where(), or())
  ```java
    @DisplayName("LastName (hyun or lee) 찾기 - or()")
    @Test
    void test2(){

        Customer findCustomers = jpaQueryFactory
                                        .selectFrom(customer)
                                        .where(customer.lastName.eq("hyun")
                                        .or(customer.lastName.eq("lee")))
                                        .fetchOne();

        assertEquals("hyun",findCustomers.getLastName());
    }
  ```
  + 결과

  <br/>
  
  ![image](https://user-images.githubusercontent.com/76584547/125478048-bd3666b6-2ba8-42d9-8ba6-fc2b4b776f74.png)

  ### 결과 조회
  ----
  + fetch() 
    + 메소드로 리스트를 조회할 수 있다. 데이터가 없으면 빈 리스트가 조회된다.

  + fetchOne() 
    + 단 한건의 결과를 조회한다.
    + 데이터가 없으면 Null 이 조회된다.
    + 결과가 둘 이상이면 com.querydsl.core.NonUniqueResultException 예외가 발생한다.

  + fetchFirst() 
    + 첫번째로 발견되는 결과를 조회할 수 있다.

  + limit(1).fetchOne() 과 동일하다.

  + fetchResults()
    + 페이징을 포함한 결과를 조회할 수 있다.

  + fetchCount() 
    + count 쿼리로 변경해서 count 수 조회가 가능하다.

  ### 정렬
  ----
  ```
    orderBy: 
      - 정렬 표현식을 이용해서 정렬 순서를 지정한다. 
      - 숫자나 문자열에 대해서는 asc()나 desc()를 사용하고, OrderSpecifier에 접근하기 위해 다른 비교 표현식을 사용한다.
  ```
  
  + lastName 오름차순 , firstName 내림차순 정렬 - order()
  ```java
      void test3(){

        List<Customer> findCustomers = jpaQueryFactory
                                        .selectFrom(customer)
                                        .orderBy(customer.lastName.asc(),customer.firstName.desc())
                                        .fetch();

        assertAll(
                ()->assertEquals("hyun",findCustomers.get(0).getLastName()),
                ()->assertEquals("kim",findCustomers.get(1).getLastName()),
                ()->assertEquals("park",findCustomers.get(2).getLastName())
        );
    }
  ```
  + 결과

  <br/>
  
  ![image](https://user-images.githubusercontent.com/76584547/125482007-eb4bf72d-8ac3-4bb3-82e8-b1e200362d13.png)



  

  참고 사이트(★) : https://github.com/Youngerjesus/Querydsl

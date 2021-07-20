## JPA

### 특징
---
```
  ㅇ 1차 캐시
  ㅇ 동일성 보장
  ㅇ 트랜잭션을 지원하는 쓰기 지연
  ㅇ 변경 감지
  ㅇ 지연 로딩
  ㅇ 단방향 
```

### 1차 캐시 / 동일성 보장
---
```
  영속성 컨텍스트에서 persist된 값을 다시 조회할 경우 메모리에 있는 1차 캐시에서 엔티티를 조회한다.
  - 영속성 컨텍스트 내부에 Map이 하나 있는데 키는 @Id로 매핑한 식별자이고, 값은 엔티티 인스턴스다.
```

+ EntityManager.find()를 호출하면 먼저 1차 캐시에서 엔티티를 찾고 만약 찾는 엔티티가 1차 캐시에 없으면 데이터베이스를 조회한다.

![image](https://user-images.githubusercontent.com/76584547/126165706-0403f1a1-1d35-4c0f-883f-275fff6292b3.png)

<br/>

+ insert 쿼리 한 번만 나가고 find에 대한 쿼리는 나가지 않는 걸 확인할 수 있다.
```java
    @DisplayName("JPA 1차 캐시와 동일성 보장")
    @Test
    void jpaTest01(){
        Member member = new Member("hyun",28);
        entityManager.persist(member);

        Member findMember1 = entityManager.find(Member.class, 1L);
        Member findMember2 = entityManager.find(Member.class, 1L);

        assertEquals(findMember1, findMember2); //true
    }
```
+ 결과

![image](https://user-images.githubusercontent.com/76584547/126166963-5b2d20ec-4f8d-45cc-8752-985f2bbb74e4.png)


### 트랜잭션을 지원하는 쓰기 지연
---
```
  - 엔티티 매니저는 트랜잭션을 커밋하기 직전까지 데이터베이스에 엔티티를 저장하지 않고 
    내부 쿼리 저장소에 INSERT SQL을 차곡차곡 모은다
  - 그 후, 트랜잭션을 커밋할 때 모아둔 쿼리를 데이터베이스에 보내는데 이것을 트랜잭션을 지원하는 "쓰기 지연"이라한다.
```
  + 쓰기 지연 SQL 저장소
  ```
    member A를 영속화하면 1차 캐시에 저장하고 "쓰기 지연 SQL 저장소"에 보관한다.
  ```
  ![image](https://user-images.githubusercontent.com/76584547/126168673-0125c8d6-75c6-48e5-8d3c-1ca6bdd1d938.png)



  + flush
  ```
    - 영속성 컨텍스트의 변경 내용을 데이터 베이스에 동기화 하는 작업이다.
    - 등록, 수정, 삭제한 엔티티를 데이터베이스에 반영한다.
    - 이렇게 동기화한 후 커밋을 해야 데이터베이스에 반영된다.
  ```
  ![image](https://user-images.githubusercontent.com/76584547/126169012-35e9daa0-6745-4912-b898-59507e82fbe3.png)


### 트랜잭션을 지원하는 쓰기 지연과 성능 최적화
---
  + 트랜잭션을 지원하는 쓰기 지연과 JDBC 배치
  ```JAVA
    insert(member1); //INSERT INTO...
    insert(member2); //INSERT INTO...
    insert(member3); //INSERT INTO...
    insert(member4); //INSERT INTO...
    insert(member5); //INSERT INTO...
    
    commit();
  ```
  + 네트워크 호출은 한 번은 단순한 메소드를 수만 번 호출하는 것보다 더 큰 비용이 든다.
    + 위 코드는 5번의 INSERT SQL과 1번의 커밋으로 총 6번 데이터베이스와 토신한다.
    + 이것을 최적화하려면 5번의 INSERT SQL을 모아서 한 번에 데이터베이스로 보내면 된다.

  + JPA 플러시 기능을 이용해 효과적으로 SQL 배치 기능을 사용할 수 있다.
    + 최대 50건씩 모아서 배치를 실행한다.
    + 같은 SQL 때만 유효하다. 
  ```java
    hibernate.jdbc.batch_size = 50
  ```
  + 이 경우는 1번(1,2,3,4), 2번(5), 3번(6,7)을 모아서 실행한다
  ```JAVA
    em.persist(new Member()); // 1
    em.persist(new Member()); // 2
    em.persist(new Member()); // 3
    em.persist(new Member()); // 4
    em.persist(new Child()); // 5, 다른연산
    em.persist(new Member()); // 6
    em.persist(new Member()); // 7
  ```

  + 데이터베이스 테이블 로우에 락 걸리는 시간 최소화(★)
  ```JAVA
    update(memberA); // UPDATE SQL A
    비즈니스로직A(); // UPDATE SQL ...
    비즈니스로직B(); // INSERT SQL ...
    commit();
  ```
  + SQL을 사용할 경우
    + UPDATE 시점부터 비즈니스로직A(), 비즈니스로직B()를 모두 실행하고 COMMIT 시점까지 해당 ROW에 락이 걸린다.
    + UPDATE후 비즈니스로직A(), 비즈니스로직B()시점에도 락 걸려있음
  + JPA의 경우 
    + COMMIT할 때 UPDATE SQL을 실행하고 데이터베이스 트랜잭션을 커밋하기 때문에 락 걸리는 시간을 최소화 할 수 있다.
    + COMMIT 시점에만 UPDATE 하므로 비즈니스 로직 A,B 때는 락이 안 걸려있다.

### 변경 감지(dirty-checking)
---
  + SQL 사용 시 
  ```
    - SQL 이용시 수정 쿼리가 많아지고, 비즈니스 로직을 분석하기 위해 SQL을 계속 확인해야한다. 
    - 결국 직접적이든 간접적이든 비즈니스 로직이 SQL에 의존하게 된다. 
  ```
  
  + 변경 감지(dirty-checking)
  ```java
      @DisplayName("변경 감지(더티체킹)")
      @Test
      void jpaTest02(){
          Member member = new Member("hyun",28);
          memberRepository.save(member);
          member.setName("kim");

          Member findMember = memberRepository.findByName("kim").get();

          assertEquals(findMember.getAge(), member.getAge());
      }
  ```
  
  ![image](https://user-images.githubusercontent.com/76584547/126337611-a891e885-81d0-4f74-8566-9788efcc6af9.png)
  
  + 스냅샷
  ```
    - JPA는 엔티티를 영속성 컨텍스트에 보관할 때, 최초 상태를 복사해서 저장해두는 데 이것을 스냅샷이라고 한다.
    - 그리고 "플러시" 시점에 스냅샷과 엔티티를 비교해서 변경된 엔티티를 찾는다.
  ```
  
  + 변경 순서
    1. 트랜잭션을 커밋하면 엔티티 매니저 내부에서 먼저 플러시(flush())가 호출된다.
    2. 엔티티와 스냅샷을 비교해서 변경된 엔티티를 찾는다.
    3. 변경된 엔티티가 있으면 수정 쿼리를 생성해서 쓰기 지연 SQL 저장소에 보낸다.
    4. 쓰기 지연 저장소의 SQL을 데이터베이스에 보낸다.
    5. 데이터베이스 트랜잭션을 커밋한다.

  + 특징
  ```
    ㅇ 변경감지는 영속성 컨텍스트가 관리하는 영속 상태의 엔티티만 적용된다.
    ㅇ JPA의 기본 전략은 엔티티의 모든 필드를 업데이트한다.
  ```
  
  + JPA의 기본 전략은 엔티티의 모든 필드를 업데이트한다.
    + 모든 필드를 사용하면 수정 쿼리가 항상 같다. 따라서 애플리케이션 로딩 시점에 수정쿼리를 미리 생성해두고 사용 가능
    + 데이터베이스에 동일한 쿼리를 보내면 데이터베이스는 이전에 한 번 파싱된 쿼리를 재사용할 수 있다.

  + 필드가 많거나 내용이 클 경우
    + 하이버네이트 확장기능인 @DynamicUpdate를 사용해야한다.

  + ※참고!!!
    + 추천하는 방법은 기본 전략을 사용하고, 최적화가 필요할 정도로 느리면 그때 전략을 수정한다.
    + 참고로 한 테이블에 컬럼이 30개 이상 된다는 것은 테이블 설계상 책임이 적절히 분리되지 않았을 가능성이 높다. 

  
  ### 단방향 연관관계 / 양방향 연관관계
  -----
  ![image](https://user-images.githubusercontent.com/76584547/126342590-9d3063a7-12e2-4d7b-8e63-8b08362ee7ff.png)
  
  + 단방향 연관관계
  ```
    ㅇ member.team 필드를 통해서 team을 알 수 있지만 team -> member는 알 수 없다.
  ```
  + 양방향 연관관계
  ```
    ㅇ 회원 테이블의 TEAM_ID 외래 키를 통해서 회원과 팀을 조인할 수 있고 반대로 팀과 회원도 조인할 수 있다.
    ㅇ MEMBER 테이블의 TEAM_ID 외래키 하나로 MEMBER JOIN TEAM과 TEAM JOIN MEMBER 둘 다 가능하다.
  ```
  
  + 예시
    + 서로 MEMBER 테이블의 TEAM_ID 외래키 하나로 MEMBER JOIN TEAM과 TEAM JOIN MEMBER 둘 다 가능한 걸 확인할 수 있다.
  ```SQL
    SELECT *
    FROM MEMBER M
    JOIN TEAM T ON M.TEAM_ID = T.TEAM_ID
  ```
  ```SQL
    SELECT *
    FROM TEAM T
    JOIN MEMBER M ON M.TEAM_ID = T.TEAM_ID
  ```
  
  + 객체 연관관계 VS 테이블 연관관계
  ```
    ㅇ 객체는 참조(주소)로 연관관계를 맺는다.
    ㅇ 테이블은 외래 키로 연관관계를 맺는다.
    
    ※ 이 둘은 비슷해보이지만 매우 다른 특징을 가진다. 연관된 데이터를 조회할 때
       객체는 참조(a.getB().getC())를 사용하지만 테이블은 조인 JOIN을 사용한다.
  ```
  + 참조를 사용하는 객체의 연관관계는 단방향이다.
    + A -> B (a.b)
  + 외래키를 사용하는 테이블의 연관관계는 양방향이다.
    + A JOIN B가 가능하면 반대로 B JOIN A도 가능하다.  
  + 객체를 양방향으로 참조하려면 단방향 연관관계를 2개 만들어야한다.
    + A -> B (a.b)
    + B -> A (b.a) 


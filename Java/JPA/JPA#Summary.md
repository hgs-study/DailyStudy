##JPA

### 특징
---
```
  ㅇ 1차 캐시
  ㅇ 동일성 보장
  ㅇ 트랜잭션을 지원하는 쓰기 지연
  ㅇ 변경 감지
  ㅇ 지연 로딩
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

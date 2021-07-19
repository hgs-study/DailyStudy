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

### 1차 캐시
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


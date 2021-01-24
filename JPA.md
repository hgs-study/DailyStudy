## JPA
```
  - 인터페이스의 모음
  - 3가지의 구현체 존재(Hibernate, EclipseLink,DataNucleus) => 대부분 하이버네이트
  - SQL 중심적인 개발에서 -> 객체 중심의 개발
  - 생산성 / 유지보수 / 패러다임의 불일치 해결 / 성능 / 데이터 추상화와 벤더 독립성 / 표준
```

+ 생산성 
```
  - 저장 : jpa.persist(member)
  - 조회 : Member member = jpa.find(memberId)
  - 수정 : member.setName("변경할 이름")
  - 삭제 : jpa.remove(member)
```

+ JPA의 성능 최적화 기능
```
  - 1차 캐시와 동일성(identity) 보장
    => 같은 트랜잭션 안에서는 같은 엔티티를 반환 - 약간의 조회 성능 향상
       ex) String memberId= "100";
           Member m1 = jpa.find(Member.class, memberId); // SQL
           Member m2 = jpa.find(Member.class, memberId); // 캐시
           m1 == m2 (같다!!!!)
           
           => SQL은 1번만 실행됨 (캐싱을 하고있음)
           
  - 트랜잭션을 지원하는 쓰기 지연(transactional write-behind) => 버퍼링
    ex) em.persist(memberA);
        em.persist(memberB);
        em.persist(memberC);
        
        //커밋하는 순간 데이터베이스에 insert sql을 모아서 보낸다.
        transaction.commit(); //커밋
        
  - 지연 로딩(Lazy Loading)
    => 지연로딩(Lazy) vs 즉시로딩(Eager)
    
      ㅇ 지연로딩
        - 엔티티 조회 시점이 아닌 엔티티 내 연관관계를 참조할 때 해당 연관관계에 대한 SQL이 질의된는 기능이며, fetch = FetchType.LAZY 옵션으로 지정할 수 있다.
        - 엔티티 조회 시, 연관관계 필드는 프록시 객체로 제공된다.
        - 쿼리가 2번 나감
        
      ㅇ 즉시로딩
        - 엔티티 조회 시 연관관계에 있는 데이터까지 한 번에 조회해오는 기능이며, fetch = FetchType.EAGER 옵션으로 지정할 수 있다.
        - 즉시 로딩으로 조회된 엔티티의 연관관계 필드에는 실제 엔티티 객체가 반환된다.
        - 쿼리가 조인해서 한 번 나감
        
      ㅇ 주의사항
        - 실무에서는 가급적 지연로딩만 사용한다.
        - 즉시로딩을 사용하면 예상치 못한 SQL이 발생할수 있기 때문.
          EX)다른 개발자가 Member를 조회할 때는 Team이 조회될 거라고 생각하지 못하기 때문에
          
      ㅇ 연관관계별로 fetch 옵션의 기본값이 다르다.
        - @ManyToOne : EAGER
        - @OneToOne : EAGER
        - @ManyToMany : LAZY
        - @OneToMANY :LAZY
```

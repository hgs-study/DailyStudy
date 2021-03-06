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
+ JPA 영속성 관리
```
  ㅇ 영속성 컨텍스트의 이점
    - 1차 캐시와 쓰기 지연 SQL 저장소를 가짐
    - 동일성(identity) 보장
    - 트랜잭션을 지원하는 쓰기 지연
    - 변경 감지
    - 지연로딩

  ㅇ 플러시
    - 변경 감지(더티체킹) - update
    - 수정된 엔티티 쓰기 지연 SQL 저장소에 등록
    - 쓰기 지연 SQL 저자소의 쿼리를 데이터 베이스랑 동기화(전송) ->등록,수정,삭제 쿼리
    - 영속성 컨텍스트를 비우지 않음
    - 트랜잭션이라는 작업 단위가 중요 -> 커밋 직전에만 동기화하면 됨

  ㅇ 준영속 상태
    - 영속 상태의 엔티티가 영속성 컨텍스트에서 분리(detached)
    - 영속성 컨텍스트가 제공하는 기능을 사용못함
    - ex) em.detached(entity), em.clear(), em.close
```

+ JPA 엔티티 매핑
```
  ㅇ 데이터베이스 스키마 자동 생성
    - <property name="hibernate.hbm2ddl.auto" value="create / create-drop / update / validate / none" /> 설정
    - create : 기존 테이블 삭제 후 다시 생성 (drop + create)
    - create-drop : create와 같으나 종료 시점에 테이블 drop
    - update : 변경분만 반영 (운영 DB에는 사용하면 안됨)
    - validate : 엔티티와 테이블이 정상 매핑되어있는지만 확인
    - none : 사용하지 않음
   
  ㅇ 주의사항
    - 개발 초기 단계 : create , update
    - 테스트 서버 : validate
    - 운영 서버 : validate, none
    => 데이터가 적재되어 있는데 create를 하면 drop 하기 때문에 테이블을 날린다.
    => update시 테이블 alter로 변경하지만 몇분간 데이터베이스 테이블 락이 걸린다.
```

+ 기본키 매핑
```
ㅇ@GeneratedValue(strategy = GenerationType)
- IDENTITY
=> em.persist시 insert 쿼리 날리고 id 반환(유일하게 persist 때 insert 쿼리 날림)
    이유: insert후 id(pk값을 얻어야 영속성 컨텍스트에 저장할 수 있기 때문) 
- SEQUENCE
=> em.persist시 SEQ next value 가져오고 em.commit 시 insert 쿼리 날림
=> allocationSize : 기본 50 -> db에 미리 50개를 올려놓고 우리가 persist할때마다 메모리에 1,2씩 쌓임 ->50개에 다다르면 그 때 다시 next value를 50개씩 가져옴(51번~100번 세팅) 
=> 1번쨰 next value 호출 : 1     |  1
=> 2번째 next value 호출 : 51    |  2
=> 3번째        "        : 51    |   3
             ... 51 만나면: 101  |  . ..
※ 여러 웹서버가 있어도 동시성 이슈 없이 다양한 문제가 해결됨

```

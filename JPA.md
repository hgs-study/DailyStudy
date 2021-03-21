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

+ 양방향 연관관계와 연관관계의 주인
```
★★★★★
ㅇ 연관관계의 주인과 mappedBy
  - mappedBy = JPA의 멘탈 붕괴 난이도
  - 객체와 테이블간에 연관관계를 맺는 차이를 이해해야한다.
  
ㅇ 객체의 양방향 관계
  - 객체의 양방향 관계는 사실 양방향 관계가 아니라 서로 다른 단방향 관계 2개다.
  - 객체를 양방향으로 참조하려면 단방향 연관관계를 2개 만들어야한다. (A,B 클래스가 있다면 A.getB(), B.getA())
  
ㅇ 연관관계의 주인(Owner)
  ※ 양방향 매핑 규칙
    - 객체의 두 관계 중 하나를 연관관계의 주인으로 지정
    - 연관관계의 주인만이 외래 키를 관리 (등록,수정)
    - 주인이 아닌 쪽은 읽기만 가능
    - 주인은 mappedBy 속성 사용 X
    - 주인이 아니면 mappedBy 속성으로 주인 

  ※ 누구를 주인으로?
    - 외래키가 있는 곳을 주인으로 정해라
    - Member, Team / @OneToMany, @ManyToOne 관계일경우
    - Member.team이 연관관계 주인
    - @OneToMany, @ManyToOne 등 관계에서 무조건 다(Many)쪽이 연관관계의 주인이 됨
    - 성능이슈, 외래키랑 같은 테이블에서 관리할 수 있음

ㅇ 양방향 매핑 시 가장 많이 하는 실수
  - mappedBy (연관관계 주인이 아닌 역방향에서 연관관계 설정)

ㅇ 양방향 매핑(★★★★★)
  - 양쪽 (주인과 역방향)에 둘 다 값을 넣어주는 것이 맞음 (순수 객체 상태를 고려해서 항상 양쪽에 값을 설정하자)
  - em.flush , clear를 하지 않고 다시 해당 객체를 조회했을 경우 1차 캐시된(메모리에만 올라가있는) 객체를 가져오기 때문에 해당 객체는 빈 객체로 문제가 된다.
  - 주인 객체에서 setTeam(역방향) 편의 메서드를 만든다. 하나만 호출해도 양쪽 다 세팅이 된다.
    => chageTeam(Team team){   (주인)
        this.team = team;
        team.getMembers.add(this)
       }
    => addOrderItem(OrderItem orderItem){
        orderItems.add(orderItem);
        orderItem.setOrder(this);
      }
  - 애플리케이션 만들 때마다 상황이 다르기 때문에 상황마다 주인에 설정할 것 것인지 역방향에 설정할 것인지 정해야한다.
  - 무한루프를 조심하자
    => 1. toString() 양쪽 다 설정하면 양쪽에서 무한으로 toString 호출   -> stackOverFlow
          => 해결: 롬복에서 toString은 거의 쓰지마라. 쓰더라도 양방향 관계는 빼고 써라.
       2. JSON 생성 라이브러리 (toString과 마찬가지로 양방향 관계일 때, Entity를 JSON으로 바꾸는 순간 무한 루프) members -> team -> members -> team -> .....
          => 해결: 컨트롤러에서 엔티티를 절대 반환하지 마라
              (이유 : 1.무한루프, 2.엔티티가 변경되는 순간 해당 api 스펙이 바뀌어버림)
 
ㅇ 양방향 매핑 정리
  - 단방향 매핑만으로도 이미 연관관계 매핑은 완료(중요, 웬만하면 단방향 매핑만으로 설계를 끝내야한다. 처음엔 무조건 단방향으로 설계를 끝내라)
  - 양방향 매핑은 반대 방향으로 조회(객체 그래프 탐색) 기능이 추가가 된 것 뿐이다!!!
  - JPQL에서 역방향으로 탐색할 일이 많음
  - 단방향 매핑을 잘하고 양방향 매핑은 필요할 때만 추가해도 됨 (테이블에 영향을 주지 않음)
 
ㅇ 양방향 VS 단방향
  - 방법 1 (양방향 연관관계)
    Order order = new Order();
    order.addOrderItem(new OrderItem());


  - 방법 2 (단방향 연관관계)
    Order order = new Order();
    em.persist(order);

    OrderItem orderItem = new OrderItem();
    orderItem.setOrder(order);
    em.persist(orderItem);
    //단방향 연관관계로도 애플리케이션을 만드는데에 지장이 없다.
    //양방향 연관관계로 하는 이유는 조회를 더 편하게 하기 위해서
    //나중에 JPQL로 조인을 더 복잡하게 해야하는데 이 작업을 더 간단히 하기위해 양방향으로 많이 짠다.
    //핵심은 단방향으로 할 수 있으면 최대한 단방향으로 해라!
    
```

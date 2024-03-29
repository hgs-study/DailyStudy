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

+ 양방향 연관관계와 연관관계의 주인 (★★★★★)
```
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
    //하지만 실무에서 사용할 때 조회를 간단히 하기 위해 양방향으로 하기 위한 작업이 많아진다.
    
```

+ 다양한 연관관계 매핑
```
ㅇ 연관관계 매핑 시 고려사항 3가지
  - 다중성 : 
    => 다대일 @ManyToOne : 실무에서 가장 많이 사용함
       일대다 @OneToMany : 필요할 때 많이 씀
       일대일 @OneToOne : 가끔나옴
       다대다 @ManyToMany : 실무에서 사용하면 안됨!!
       
  - 단방향, 양방향
    => ㅇ 테이블 
          - 외래키 하나로 양쪽으로 조인 가능
          - 사실 방향이라는 개념이 없음
       ㅇ 객체
          - 참조용 필드가 있는 쪽으로만 참조 가능 (한쪽 - 단방향 , 양쪽 - 양방향)
          - 한쪽만 참조하면 단방향
          - 양쪽이 서로 참조하면 양방향 (사실 양방향이라는 것은 없고 단방향이 2개)
          
  - 연관관계 주인
    => - 테이블은 외래 키 하나로 두 테이블이 연관관계를 맺음
       - 객체 양방향 관계는 A->B, B->A 참조가 2개
       - 객체 양방향 관계는 참조가 2군데 있음. 둘 중 테이블의 외래 키를 관리할 곳을 지정해야함
       - 연관관계의 주인 : 외래키를 관리하는 곳
       - 주인의 반대편(역방향) : 외래 키에 영향을 주지 않음, 단순 조회만

ㅇ 다대일[N:1] 
  - 항상 다(N)쪽에 FK가 있어야함(EX: TEAM <-> MEMBER 일 경우, MEMBER에 FK 보유)
  - MEMBER가 연관관계 주인
  - 가장 많이 사용, 일대다의 반대
  - 양쪽을 서로 참조할 때 필요

ㅇ 일대다[1:N]
  - 일이 연관관계 주인이 되는 것
  - 이 모델은 김영한님은 권장하지 않음. (실무에서 거의 사용하지 않음!!)
  - 일대다 양방향 정리 @JoinColumn(name="TEAM_ID",insertable=false,updatable=false)
  - 실무에선 테이블이 몇십개가 엮어서 돌아가는데 운영하기 힘들어짐 (Team 객체를 만졌는데 Update Member 가 나감)
  - 다[N] 테이블에 update 쿼리가 한 번 더 나가야한다.
  - 해결 : Member 다대일을 양방향 관계로 바꿔서 가면 됨, 다대일 양방향을 사용하자 (★)

ㅇ 일대일[1:1]
  - 일대일 관계는 그 반대도 일대일
  - 주 테이블이나 대상 테이블에 둘 다 외래키를 선택해서 넣어도 된다.(어디에 넣든 상관없음)
  - 외래키에 데이터베이스 유니크(UNI) 제약조건이 추가되어야 가능 
  - 외래키가 대상 테이블에 있을경우 -> 양방향을 걸고 조회한다.
  
  - 주 테이블에 외래키 (영한님은 이 방법을 선호하심 - DBA분들이 싫어할 수 있음) 
    ㅇ 객체지향 개발자 선호 / JPA 매핑 편리
    ㅇ 장점 : 주 테이블만 조회해도 대상 테이블에 데이터가 있는지 확인 가능
    ㅇ 단점 : 값이 없으면 외래 키에 NULL 허용
    
  - 대상 테이블에 외래키
    ㅇ 전통적인 데이터베이스 개발자 선호
    ㅇ 장점 : 주 테이블과 대상 테이블을 일대일에서 일대다 관계로 변경할 때 테이블 구조 유지
    ㅇ 단점 : 프록시 기능의 한계로 지연 로딩으로 설정해도 항상 즉시 로딩됨 (대상 테이블이 있는지 없는지 먼저 확인해봐야하기 때문에 WHERE문으로 확인함(쿼리가 먼저 나감))
    
ㅇ 다대다 [N:M]
  - 영한님은 실무에서 절대 사용하지 않는다!!
  - 객체는 다대다 관계가 가능하지만 관계형 데이터베이스는 중간에 테이블을 하나 두고 일대다 다대일로 풀어내야함(MEMBER <-> MEMBER_PRODUCT <-> PRODUCT)
  - 편리해보이지만 연결테이블(MEMBER_PRODUCT)에 추가 정보를 넣을 수 없음
  => 극복
      ㅇ 연결 테이블용 엔티티 추가 (연결테이블을 엔티티로 승격)
        - @ManyToMany -> @OneToMany <-> @ManyToOne @ManyToOne <-> @OneToMany 
        - ORDER 엔티티생성 -> ORDER(DB)
  - PK를 GeneratedValue / PK,FK 조합 2가지로 설정할 수 있다 하지만 김영한님은 GeneratedValue로 PK를 계속 가져가는게 애플리케이션을 계속 운영할 때 유연하다. PK,FK의 조합은 어디에 종속적이여서 묶여 있고 유연하지 못하다.
```

+ 고급 매핑
```
ㅇ 상속관계 매핑
ㅇ @MappedSuperclass
```
  + 상속관계 매핑
    + 관계형 데이터 베이스는 상속 관계 x <-> 객체는 o
    + 슈퍼 타입, 서브타입 관계라는 모델링 기법이 객체 상속과 유사
    + 상속관계 매핑 : 객체의 상속 구조와 DB의 슈퍼타입, 서브타입 관계를 매핑
       + 슈퍼타입 : 물품
         서브타입 : 음반, 영화, 책
       + 객체는 상속 - Item
               자식 - Album, movie , book
    + 슈퍼타입 브타입 논리 모델을 실제 물리 모델로 구현하는 3가지 방법
      + 조인 전략 - 테이블을 각각 조인 item_id로 insert를 각각 테이블에 2번 함 / JPA랑 가장 유사한 모델
        => 상속 관계일 경우 자식 엔티티 조회시 JPA가 부모를 자동으로 조인해줌
      + 단일 테이블 전략 :  논리 모델을 한 테이블로 합치는 것 (Item에 album, movie, book 컬럼을 다 넣음) (JPA 기본 전략)
      + 구현 클래스마다 테이블 전략 : album,movie,book에 item_id를 다 가지고 있음
      
    + DB의 논리 모델링 3가지를 어떤 것으로 구현을 하든 JPA는 다 지원을 해줌 
      + 기본 전략은 싱글 테이블 전략(한 테이블에 다 떄려박음)

    + 조인 전략 (JOINED) 정석★
      + @Inheritance(strategy = InheritanceType.JOINED)
      + @DiscriminatorColumn (DTYPE) - 엔티티타입 (부모엔티티에 넣어주면 조회하기 좋음)
      + @DiscriminatorValue(name="")를 활용해 자식 엔티티의 이름을 지정할 수 있다.
      + 장점 : 
        1. 정규화가 되어있음
        2. 외래키 참조 무결설 제약 조건 가능 (아예 다른 테이블에서 조회할때 부모(item)클래스만 보면 됨)
        3. 저장 공간 효율
      + 단점 :
        1. 조회 시 조인을 많이 사용 / 성능 저하
        2. 조회 쿼리가 복잡
        3. 데이터 저장시 SQL 2번 호출
        (하지만 이 모든 것이 그렇게 크게 영향을 미치지 않는다.)

    + 단일 테이블 (SINGLE_TABLE)
      + @Inheritance(strategy = InheritanceType.SINGLE_TABLE)
      + 한 테이블에 다 때려 넣고 실행
      + @DiscriminatorColumn 꼭 넣어줘야함(필수) DTYPE으로 구분하기 때문 -> @DiscriminatorColumn이 없어도 자동으로 DTYPE이 들어감
      + 부모 테이블에 전체 데이터가 들어감 (자식테이블은 하나도 안 들어감)
      + 성능은 제일 잘 나옴
      + 장점 : 
        1. 조인이 필요 없어서 성능이 빠름
        2. 조회 쿼리가 단순함
      + 단점 :
        1. 자식 엔티티가 매핑한 컬럼은 모두 NULL 허용
        2. 단일 테이블에 모든 것을 저장하므로 테이블이 커지고 조회 성능이 오히려 느려질 수 있다.

    + 구현 클래스마다 테이블 전략
      + @Inheritance(strategy = InheritanceType.TABLE_PER_CLASS) 
      + item 테이블을 없애고 album,movie,book 테이블에 각각 다 name,price 필드를 추가
      + 부모(item)클래스는 abstract(추상클래스)로 설정
      + @DiscriminatorColumn 쓸모 없음
      + 부모 객체로 찾을 때 Union All로 테이블을 다 뒤짐(상당히 복잡한 쿼리)
      + 절대 사용하면 안되는 전략!!! (개발자,DBA 둘 다 싫어함 겹치는 게 없음)
      + 장점 :
        1. 서브 타입을 명확하게 구분해서 처리할 때 효과적
        2. NOT NULL 제약조건 사용 가능
      + 단점 :
        1. 여러 자식 테이블 조회할때 UNION 성능 느림
        2. 자식 테이블 통합해서 쿼리하기 어려움

    + ※결론
      + 단순하면 단일테이블
      + 비즈니스 로직, 복잡하면 조인전략

  + @MappedSuperClass
    + 공통적 매핑 정보가 필요할 때
    + 매핑 정보를 받는 슈퍼 클래스
    + 생성 날짜, 수정 날짜 등 -> 하지만 이 생성 정보, 날짜들은 JPA에서 기본적으로 제공해주는 것을 사용 (JPA에 이벤트가 있어서 영속하기 직전에 어떤걸 호출해! 이게 가능하다)
    + 엔티티가 X , 테이블과 매핑 X 
    + 자식 클래스에 정보만 제공
    + 직접 사용할 일이 없으므로 추상클래스로 사용하는 것을 권장


+ 프록시
```
ㅇ 프록시 기초
ㅇ 프록시 특징
ㅇ 프록시 객체의 초기화
ㅇ 프록시 중요 특징 ★
ㅇ 프록시 확인
```
  + 프록시 기초
    + em.find() vs em.getReference()
      + em.find() : 데이터베이스를 통해서 실제 엔티티 객체 조회
      + em.getReference() : 데이터베이스 조회를 미루는 가짜(프록세) 엔티티 객체 조회 
        -> em.getReference를 호출하는 시점에서는 쿼리를 안날림, 해당 객체를 사용하는 시점엔 쿼리 나감
       
  + 프록시 특징
    + 하이버네이트가 어떤 내부 라이브러리를 사용해서 프록시 객체 설정 
    + 실제 클래스를 상속 받아서 만들어진다.
    + 실제 클래스와 겉모양이 같다.
    + 사용하는 입장에서는 진짜 객체인지 프록시 객체인지 구분하지 않고 사용하면 됨(이론상)
    + 프록시 객체는 실제 객체의 참조(target)를 보관
    + 프록시 객체를 호출하면 프록시 객체는 실제 객체의 메소드 호출
    
  + 프록시 객체의 초기화
    ```
     Member member = em.getReference(Member.class,"id1");
      member.getName();  
    ```
      1. em.getReference()로 프록시 객체를 가져온 다음에, getName() 메서드를 호출
      2. MemberProxy 객체에 처음에 target 값이 존재하지 않는다. JPA가 영속성 컨텍스트에 초기화 요청을 한다.
      3. 영속성 컨텍스트가 DB에서 조회
      4. 실제 Entity를 생성
      5. 프록시 객체가 가지고 있는 target(실제 Member)의 getName()을 호출해서 결국 member.getName()을 호출한 결과를 받을 수 있다.
      6. 프록시 객체에 target이 할당 되고 나면, 더이상 프록시 객체의 초기화 동작은 없어도 된다.
      실제로는 위와 같이 프록시 객체가 동작 한 것이다.
        
  + 프록시 중요 특징 ★
    + 프록시 객체는 처음 사용할 때 한 번만 초기화
    + 프록시 객체를 초기화 할 때, 프록시 객체가 실제 엔티티로 바뀌는 것은 아님!!!! 초기화 되면 프록시 객체를 통해 실제 엔티티에 접근 가능
    + 프록시 객체는 원본 엔티티를 상속 받음, 따라서 타입 체크시 주의해야함( 타입 비교 시 == 비교 실패, 대신 instanceof 사용) ★
    + 영속성 컨텍스트에 찾는 엔티티가 이미 있으면 em.getReference()를 호출해도 실제 엔티티를 반환
      => find(실제 엔티티)로 가져오든 reference(가짜 엔티티)로 가져오든 ==비교하면 무조건 true 가 나와야 하기 때문 ★
      => proxy를 먼저 올리고 find하면 둘 다 proxy 객체
      => find 먼저 올리고 proxy 가져오면 둘 다 실제 Member 객체
      => JPA 한 트랜잭션에 객체 타입이 같음을 보장해준다!! ★★
    + 영속성 컨텍스트의 도움을 받을 수 없는 준영속 상태일 때, 프록시를 초기화할 때 문제 발생 (em.detach) ★★
      => (하이버네이트는 org.hibernate.LazyInitializationException
    
  + 프록시 확인
    + 프록시 인스턴스 초기화 여부 확인
      + PersistenceUnitUtil.isLoaded(Object entity)
    + 프록시 클래스 확인 방법
      + entity.getClass().getName() 출력(..javasist.. or HibernateProxy..)  => proxy.getClass()
    + 프록시 강제 초기화
      + org.hibernate.Hibernate.initialize(entity);
    + 참고 : JPA 표준은 강제 초기화 없음
      + 강제 호출 : member.getName()   


+ 즉시로딩, 지연로딩
```
ㅇ 지연로딩 LAZY 
ㅇ 즉시로딩 EAGER
ㅇ 지연로딩과 즉시로딩 주의 ★★★★★
ㅇ 지연로딩 활용 - 실무
```
  + 지연로딩 LAZY
    + 비즈니스 로직 상 MEMBER와 TEAM을 같이 사용하지 않을 때 성능상 유리
    + fetch = FetchType.LAZY 
    + 지연로딩된 객체를 실제 사용할 시점에 DB초기화
    + 프록시 객체가 나감

  + 즉시로딩 EAGER
    + MEMBER와 TEAM을 같이 많이 사용할 때, MEMBER와 TEAM을 조인해서 조회함 (하지만 실무에선 전부 다 지연로딩 사용!!)
    + fetch = FetchType.EAGER 
    + 즉시로딩하기 때문에 프록시 객체가 아닌 진짜 객체가 나옴

  + 지연로딩과 즉시로딩 주의 ★★★★★
    + 가급적 지연 로딩(LAZY)만 사용(특히 실무에서)
    + 즉시 로딩을 적용하면 예상치 못한 SQL이 발생 (find할 때 조인된 테이블이 다 조인돼서 조회됨(나도 모르는 쿼리가 나옴))
    + 즉시 로딩은 JPQL에서 N+1 문제를 일으킨다.
      + JPQL은 쿼리문을 그대로 SQL로 번역이 된다. 따라서 MEMBER만 셀렉트되고 EAGER이기 때문에 TEAM도 셀렉트함
      + 따라서 MEMBER 조회 1번 TEAM 조회 1번 => 총 2번 셀렉트문이 나감
      + N + 1 이란 조인된 TEAM N개  + 최초 쿼리 MEMBER 1개 
      + 해결 3가지 방법 (일단 지연로딩으로 발라야 됨)
        1. 패치조인 (필요할 때 join fetch로 조인해서 가져온다) - 대부분 이것으로 다 해결
        2. 엔티티 그래프, 어노테이션
        3. 배치 사이즈 
    + @ManyToOne, @OneToOne은 기본이 즉시로딩(EAGER)
      + LAZY로 설정
    + @OneToMany, @ManyToMany는 기본이 지연로딩(LAZY)

  + 지연로딩 활용 - 실무 ★
    + 모든 연관관계에 지연로딩을 사용해라!!
    + 실무에서 즉시로딩을 사용하지마라!!
    + JPQL fetch 조인이나 엔티티 그래프 기능을 사용해라!
    + 즉시로딩은 상상하지 못한 쿼리가 나간다 
    
    
+ 영속성 전이(CASCADE)
```
ㅇ 영속성 전이란?
ㅇ 주의사항
ㅇ 종류
```
  + 영속성 전이란(CASCADE)?
    + 특정 엔티티를 영속 상태로 만들 때 [연관된 엔티티도 함께 영속 상태]로 만들고 싶을 때
    + 연관관계인 엔티티도 persist 날려줄거야!
    + 예) 부모 엔티티를 저장할 때 자식 엔티티도 함께 저장
    + 실무에서 많이 씀

  + 주의사항
    + 영속성 전이는 연관관계 매핑하는 것과는 아무 관련이 없음
    + 엔티티를 영속화할 때 연관된 엔티티도 함께 영속화하는 편리함을 제공할 뿐
    
  + 종류 (이 세가지만 자주 씀)
    + ALL : 모두 적용 (영속 + 삭제)
    + PERSIST : 영속 (삭제하면 안될때)
    + REMOVE : 삭제 

  + 영한님이 느낀 점
    + 하나의 부모가 자식들을 관리할 때 의미가 있음
    + 게시판이랑 첨부파일 데이터, 경로는 사용할 수 있음 사용 O
    + 파일을 다른 엔티티에서 관리하고 여러 엔티티에서 관리할 경우 사용 X
    + 단일 엔티티에 종속적일 때 사용해야함!!
    + 두 엔티티의 라이프 사이클이 동일할 때, 단일 소유자일 경우 사용

+ 고아 객체
```
ㅇ 고아 객체란?
ㅇ 주의사항
```
  + 고아 객체란?
    + 고아 객체 제거: 부모 엔티티와 연관관계가 끊어진 자식 엔티티를 자동으로 삭제
    + orthpanRemoval = true
    + orthpanRemoval = true일 때, findParent.getChildList().remove(0); 하면 delete쿼리 나감, false일 때는 delete 쿼리 안 나감 (컬렉션에서 빠지면 삭제 됨)
  
  + 주의사항
    + 참조가 제거된 엔티티는 다른 곳에서 참조하지 않는 고아 객체로 보고 삭제하는 기능
    + 참조하는 곳이 하나일 때 사용해야함 ★
    + 특정 엔티티가 개인 소유할 때 사용
    + @OneTtoOne, @OneToMany만 사용
    + 참고 : 개념적으로 부모를 제거하면 자식은 고아가 된다. 따라서 고아 객체 제거 기능을 활성화 하면, 부모를 제거할 때 자식도 함께 제거된다. 이것은 CascadeType.REMOVE처럼 동작한다.


  + 영속성 전이 + 고아 객체 생명주기
    + CascadeType.ALL + orphanRemoval = true
    + 스스로 생명주기를 관리하는 엔티티는 em.persist()로 영속화, em.remove()로 제거
    + 두 옵션을 모두 활성화 하면 부모 엔티티를 통해서 자식의 생명 주기도 관리할 수 있음 (Parent는 영속성 컨텍스트가 관리하지만, Child는 Parent가 관리함 ,Parent는 Dao나 Repository가 없어도 됨)
    + 도메인 주도 설계(DDD)의 Aggregate Root개념을 구현할 때 유용 (레파지토리는 Aggregate Root(Parent)만 접근함, child는 Aggregate Root의 접근으로만 생명주기를 관리함)



+ 연관관계 정리
```
ㅇ 글로벌 페치 전략 설정
ㅇ 영속성 전이 설정
```
  + 글로벌 페치 전략 설정 ★
    + 모든 연관관계를 지연 로딩으로!!!
    + @ManyToOne, @OneToOne은 기본이 즉시로딩이므로 지연 로딩으로 변경 

  + 영속성 전이 설정
    + Order -> Delivery 영속성 전이 ALL  -> 주문을 하는 동시에 배달 엔티티의 라이프 사이클을 맞추겠다
    + Order -> OrderItem 영속성 전이 ALL 

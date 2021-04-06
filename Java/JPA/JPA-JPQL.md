+ 객체지향 쿼리 언어(JPQL)
```
ㅇ 소개
```
ㅇ 소개
+ JPA는 다양한 쿼리 방법을 지원
  + JPA
  + JPA Criteria
  + QueryDSL
  + 네이티브 SQL
  + JDBC API 직접 사용, Mybatis, SpringJdbcTemplate 함께 사용
  
+ JPQL
  + JPA를 사용하면 엔티티 객체를 중심으로 개발
  + 문제는 검색 쿼리
  + 검색을 할 때도 테이블이 아닌 엔티티 객체를 대상으로 검색
  + 모든 DB데이터를 객체로 변환해서 검색하는 것은 불가능
  + 애플리케이션이 필요한 데이터만 DB에서 불러 오려면 결국 검색 조건이 포함된 SQL이 필요
  
  + JPA SQL을 추상화한 JPQSL이라는 객체 지향 쿼리 언어 제공
  + JPA : 객체 대상 쿼리
  + SQL : 테이블 대상 쿼리
  
+ Criteria
  + 동적 쿼리 만들기 용이
  + where1111, frommm 등 컴파일 에러 확인 가능
  + 하지만 알아보기 너무 힘들다.
  + 영한님은 실무에서 아예 안쓰심
  + 너무 복잡하고 실용성이 없다.
  + ※ Criteria 말고 QueryDSL 사용 권장
  
+ QueryDSL 소개
  + 문자가 아닌 자바코드로 JPQL을 작성할 수 있음
  + JPQL 빌더 역할
  + 컴파일 시점에 문법 오류를 찾을 수 있음
  + 동적쿼리 작성 편리함
  + 단순하고 쉬움
  + 실무 사용 권장!!!
  + 기승전 JPQL!! JPQL을 잘하고 잘 알면 QueryDSL은 도큐 보면서 하면 금방 익힘

+ 네이티브 SQL
  + JPQ가 제공하는 SQL을 직접 사용하는 기능
  + JPQL로 해결할 수 없는 특정 데이터베이스에 의존적인 기능
  + EX) 오라클 CONNECT BY, 특정 DB만 사용하는 SQL힌트
  
+ JDBC 직접 사용, SpringJdbcTemplate 사용
  + JPA를 사용하면서 JDBC 커넥션을 직접 사용하거나, 스프링 JdbcTemplate, 마이바티스 등을 함께 사용 가능
  + 단 영속성 컨텍스트를 적절한 시점에 강제로 플러시 필요!!!
  + 예) JPA를 우회해서 SQL을 실행하기 직전에 영속성 컨텍스트 수동 플러시!!!!! ★
  + 영한님은 95% JPQL/QueryDSL로 사용하고 나머지 정말 복잡한 5%는 SpringJdbcTemplate 사용

-------------
+ JPQL
  + 개념, getResultList, getSingleResult 부분 날림......... (jpql-shop -> commit 참고)

  + 프로젝션
    + SELECT 절에 조회할 대상을 지정하는 것  
    + 대상 : 엔티티, 임베디드 타입, 스칼라 타입(숫자, 문자 등 기본 데이터 타입)
    
    + SELECT m FROM Member m -> 엔티티 프로젝션 (m)  (엔티티 프로젝션는 다 영속성 컨텍스트에서 관리함)
    + SELECT m.team FROM Member m -> 엔티티 프로젝션 (조인)
    + SELECT m.address FROM Member m -> 임베디드 타입 프로젝션
    + SELECT m.username, m.age FROM Member -> 스칼라 타입 프로젝션 (sql짜듯이 내가 가지고 싶어오고 싶은 것 가져옴)
    + DISTINCT로 중복 제거
  
  + 프로젝션 - 여러 값 조회 (반환값이 String,int 등등 타입이 다름 ex)username,age 등
    + Query 타입으로 조회
    + Object[] 타입으로 조회
    + new 명령어로 조회 (표준)
      + 단순 값을 DTO로 바로 조회  
      + 순서와 타입 일치해야함
      + DTO로 뽑아낼 때는 new jpql.MemberDTO(m.username,m.age)로 작성
      + 나중에 쿼리DSL 사용하면 이 부분도 극복이 됨

  + 페이징 API
    + 페이징을 다음 두 API로 추상화
    + setFirstResult(int startPosition) : 조회 시작 위치(0부터 시작)
    + setMaxResults(int maxResult) : 조회할 데이터 수
    + hibernate.dialect 을 변경하더라도 변경한 DB의 페이징 폼으로 나감
  
  + 조인
    + 내부 조인 (INNER), 외부 조인 (OUTER), 세타 조인(연관관계 없는 거 비교할 때)
    + ON절 
      + ON절을 활용한 조인(JPA 2.1~)
        + 조인 대상 필터링
        + 연관관계 없는 엔티티 외부 조인(Hibernate 5.1~)
         
  + 서브쿼리
    + 서브쿼리 지원
    + 지원 함수
      + EXISTS
      + ALL
      + ANY
      + IN 
    + JPA 서브 쿼리 한계
      + JPA 표준 스펙에서는 WHERE,HAVING 절에서만 서브쿼리 사용 가능
      + SELECT 절도 가능 (하이버네티으에서 지원)
      + FROM 절의 서브 쿼리는 현재 JPQL에서 불가능 !!!(조인으로 풀 수 있으면 풀어서 해결)
        + 네이티브 SQL
        + 결과를 가져와서 APP에서 조립하는식으로 사용(김영한님은 주로 이런 방식, 네이티브는 잘 안 씀)

  + JPQL 타입 표현
    + 문자 : 'Hello', 'She"s' 
    + 숫자 : 10L(Long), 10D(Double), 10F(Float)
    + Boolean : TRUE, FALSE
    + ENUM : jpabook.MemberType.Admin (패키지명 포함) 조심!!
    + 엔티티 타입 : Type(m) = Member (상속관계에서 사용)

  + JSQL 기타 표현
    + EXISTS, IN
    + AND, OR, NOT
    + = , > , >= , < , <= <>
    + BETWEEN, LIKE, IS NULL

  + 조건식 CASE식 (분기)
    + case when ~ then ~ else end
    + COALESCE : 하나씩 조회해서 NULL이 아니면 반환 -> coalesce(m.username,'이름 없는 회원') // 이름이 null이면 '이름 없는 회원' 반환
    + NULLIF : 두 값이 같으면 NULL 반환, 다르면 첫번째 값 반환 -> nullif(m.username,'관리자') // 이름이 '관리자'일 경우 null 반환

  + 기본 함수
    + CONCAT
    + SUBSTRING
    + TRIM
    + LOWER, UPPER
    + LENGTH
    + LOCATE
    + ABS, SQRT, MOD
    + SIZE, INDEX(JPA 용도) 
    + 기본적으로 DB다이어렉에 다 등록이 되어 있다
    + 하지만 DB를 바꿀 경우 안 될 가능성이 높다. 파라미터가 다른 등등

  + 사용자 정의 함수
    + 하이버네이트는 사용 전 방언에 추가해야한다.
      + 사용하는 DB방언을 상속받고, 사용자 정의 함수를 등록한다.
      + select function('group_concat', i.name) from Item i  
      
    + H2Dialect를 상속받아서 MyH2Dialect(커스텀 다이어렉)을 만들고 function 정의 후 사용
      +  registerFunction("group_concat", new StandardSQLFunction("group_concat", StandardBasicTypes.STRING));

-------------------
+ 객체 지향 쿼리 언어 (중급)
  + 경로 표현식 용어 정리
    + 상태필드 : 단순히 값을 저장하기 위한 필드 (m.username)
    + 연관필드 : 연관관계를 위한 필드
      + 단일값 연관필드 : @ManyToOne, @OneToOne, 대상이 엔티티(ex : m.team)
      + 컬렉션 값 연관필드 : @OneToMany, @ManyToMany, 대상이 컬렉션 (ex : m.orders)  

  + 경로 표현식 특징
    + 상태필드 : 경로 탐색의 끝, 탐색x //m.username
    + 단일값 연관 경로 : 묵시적 내부 조인(inner join)발생, 탐색o //m.team.name
      ※ 웬만하면 묵시적 조인이 일어나게 짜면 안된다!!!!(김영한님)
    + 컬렉션 값 연관 경로 : 묵시적 내부 조인 발생 , 탐색 x  //t.members  (객체그래프 탐색 x) ★
      + FROM절에서 명시적 조인을 통해 별칭을 얻으면 별칭을 통해 탐색 가능 
    + ※중요!! 묵시적 조인은 아예 실무에서 사용하지 말아라 !!!! ->명시적 조인을 써야 실제 쿼리 튜닝하기 쉬움
    
  + 명시적 조인, 묵시적 조인
    + 명시적 조인 : join 키워드 직접사용
    + 묵시적 조인 : 경로 표현식에 의해 묵시적으로 SQL 조인발생 (SELECT M.TEAM)

  + 실무 조언
    + 가급적 묵시적 조인 대신에 명시적 조인 사용
    + 조인은 SQL 튜닝에 중요 포인트
    + 묵시적 조인은 조인이 일어나는 상황을 한 눈에 파악하기 어려움 
-----------
+ 패치 조인 (중요 ★★★★★ - 실무에서 굉장히 중요함)
  + 패치조인이란?
    + SQL 조인 종류 X
    + JPQL에서 성능 최적화를 위해 제공하는 기능
    + 연관된 엔티티나 컬렉션을 SQL 한번에 함께 조횐하는 기능
    + JOIN FETCH 명령어 사용

  + 엔티티 패치조인
    + 회원을 조회하면서 연관된 팀도 함께 조회(SQL 한번에) 
    + SQL을 보면 회원뿐만아니라 팀(T.*)도 함께 SELECT
    + [JPQL] =  select m from Member m join fetch m.team
    + [SQL]  =  SELECT M.*, T.* FROM MEMBER M INNER JOIN TEAM T ON M.TEAM_ID = T.ID\
    


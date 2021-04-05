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
  

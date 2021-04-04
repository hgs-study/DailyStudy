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

  
  

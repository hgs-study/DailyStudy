## DynamoDB

### DynamoDB란?
---
  + NoSQL(Not Only SQL) 데이터베이스
  + 매우 빠른 쿼리 속도
  + Auto-Scaling 기능 탑재
  + Key-Value 데이터 모델 지원
  + 테이블 생성 시 스키마 필요없음
  + 모바일, 웹, IoT 데이터 사용 시 추천됨
  + SSD 스토리지 사용

### Primary Keys(PK)
---
  + PK를 가지고 데이터를 가져온다.
  + DynamoDB에는 두가지의 PK 유형이 있음 (PK, 복합키(PK + SortKey))
    + "파티션 키(Partition Key)"
      + 고유 특징(Unique Attribute)
      + 실제 데이터가 들어가는 위치를 결정해줌
      + 파티션 키 사용 시 동일한 두개의 데이터가 같은 위치에 저장될 수 없음

  + PK로 데이터 삽입
    + 데이터가 들어오면 어디로 저장될지 파티션 키 내부의 해시함수를 돌려서 그 결과를 반환한다.
    + 그 주소값(해쉬값)은 데이터가 들어갈 주소가 된다.


### 복합키(Composite Key)
---
  + 파티션키(Partition Key) + 정렬키(SortKey)
  + 예시 : 똑같은 고객이 다른 날짜에 다른 물건을 구매
  + 파티션키: 고객 아이디, 정렬키: 날짜(Timestamp)
  + 같은 파티션키의 데이터들은 같은 장소에 보관, 그 다음 정렬키에 의해 데이터가 정렬됨


### 인덱스
---
  + 다이나모 DB엔 두가지 인덱스 유형이 존재
    + LSI (Local Secondary Index)
    + GSI (Global Secondary Index)
  
  + LSI
    + 테이블 생성시에만 정의해줄 수 있음
    + 따라서 테이블 생성 후 변경, 삭제 불가능
    + 똑같은 파티션키 사용, 그러나 다른 정렬키 사용
    + 각 테이블당 최대 5개

  + GSI
    + 테이블 생성후에도 추가,변경,삭제 가능
    + 다른 파티션키, 정렬키 사용   
    + 각 테이블당 최대 20개
  
  + 프로젝션
    + 보조 인덱스는 키가 아닌 일반 속성도 포함할 수 있는데, 이러한 키가 아닌 일반 속성을 "프로젝션"이라고 한다.
    + 프로젝션이 늘면 당연히 인덱스의 크기도 늘어난다.
    + 프로젝션 유형
      + KEYS_ONLY : 키만 있고, 프로젝션 없음
      + INCLUDE : 일부 프로젝션만 포함
      + ALL : 모든 속성이 프로젝션 되어 있음.

### 데이터를 읽어오는 방법 (Query, Scan)
---
+ Query
  + Primary Key를 사용하여 데이터 검색
  + Query 사용 시 모든 데이터(컬럼) 변환
  + ProjecttionExpression 파라미터 (일종의 필터의 역할 담당)

+ Scan
  + 모든 데이터를 불러옴 (Primary Key 사용 x)
  + ProjectionExpression 파라미터

+ Query vs Scan
  + Query가 Scan보다 훨씬 효율적임
  + 따라서 Query 사용 추천


### DAX (DynamoDB Accelarator)
---
+ DAX란?
  + 클러스터 인메모리 캐시
  + 10배 이사의 속도 향상
  + 읽기 요청만 해당사항 (쓰기요청 X)
  + Black Friday날 쇼핑 웹 사이트 운영 (수많은 읽기 요청 예상) 

+ 원리
  + DAX 캐싱 시스템
    + 테이블에 데이터 삽입 & 업데이트시 DAX에도 반영
  + 읽기 요청에 맞는 데이터가 DAX에 들어있을시 DAX에서 데이터를 즉시 반환(Cache Hit) <-> (Cache Miss) 
  + Cache Hit : 찾고자 하는 데이터가 캐시에 들어있을 경우 ( <-> cache miss)

+ DAX의 단점
  + 쓰기 요청이 많은 어플리케이션에서는 부적절함
  + 읽기 요청이 많지 않은 어플리케이션에서 부적절함
  + 아직 모든 지역에서 제공하지 않음

### 쿼리 표현식
---
+ 참고
```
  https://catnap-jo.tistory.com/83
```

### 참고
---
```
  https://aerocode.net/298
```

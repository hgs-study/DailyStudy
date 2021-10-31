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

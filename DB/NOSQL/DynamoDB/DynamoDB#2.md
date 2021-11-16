## DynamoDB#2

### 설계시 주의사항 
---
  1. GSI가 5개 이상이면 RDB를 고려해봐야한다 (너무 많아지면 안됨) 
  2. LSI는 테이블 생성할 때 파티션키를 이미 가지고 있기 때문에 이미 하나 가지고 있는 것
  3. Dynamo는 RDBMS와 다르게 쿼리 기반으로 설계해야한다.
  4. 제일 처음 ERD 관계를 알아야한다!! (개발자는 이미 RDBMS ERD가 기본적인 베이스기 때문에)
  5. GSI Hashkey가 sk를 갖고 SortKey는 pk를 가질 수 있다


### 파티션간 Relation
---
  1. primary 테이블을 USER로 잡고 
  2. NEWS, DEVICE, TOKEN과 일대다 관계를 맺는다고 가정할 경우
  3. 각각 테이블의 prefix를 설정(USER#, NEWS# )
  4. USER는 PK,SK - USER#001
  5. 각 나머지 다 테이블은 PK - NEWS#001, SK- USER#001로 릴레이션 맺음
  ![관계 매핑_2](https://user-images.githubusercontent.com/76584547/141109756-c1186ab9-0477-40a0-a7e8-e8f6fa732325.png)


### 설계 순서
---
1. ERD 작성 
  => 개발자는 RDBMS에 익숙해져있기 때문에 ERD로 먼저 흐름을 알 수 있다.
2. Access Pattern 정의 
  => 엔티티별로 데이터를 어떻게 Access할 것인지 정의를 내려야한다.
  => 쿼리 기반이기 때문에 이 파트가 굉장히 중요함 (<-> RDBMS는 데이터 기반)
3. DynamoDB 테이블 설계
4. PK SK 정의
5. 인덱스 정의 (GSI)
6. DynamoDB 정의서 설계

```
  NoSQL은 ERD과 문서 작성을 열심히 신경 써야한다
```

### NoSQL CAP 중 AP에 속함
---
 + DynamoDB
    + DynamoDB는 aws에서 지원하는 key/Value의 NoSQL로 AP에 속한다고 알려져있습니다. Dynamo는 key값을 hashing 하고 또 이 값을 mod하여 맞는 서버에 저장해둡니다. 그리고 전체 노드 중 일부 node에 복제해 둡니다. 그리고 data versioning도 진행합니다. DynamoDB에는 strongly consistent 설정을 할 수 있습니다. 이 설정을 하게 되면 DynamoDB는 CP로 변하게 됩니다. 이 설정을 하게 되면 가장 최신의 데이터를 반드시 리턴하게 됩니다. 따라서 모든 노드를 찾기 때문에 가용성은 조금 떨어지게 됩니다.
 + 참고
 ```
  ㅇ https://sjh836.tistory.com/97
  ㅇ https://sabarada.tistory.com/91
 ```

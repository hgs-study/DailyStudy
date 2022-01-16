## Database
----
```
  ㅇ 정규화
  ㅇ 인덱스 장단점
```
### 정규화

#### 1정규화
----
```
  "원자성"
  모든 속성은 하나의 값만 가져야한다.
```
![image](https://user-images.githubusercontent.com/76584547/131613452-32c05691-d6d8-43e5-a5c5-ead01b881226.png)

+ 정규화
![image](https://user-images.githubusercontent.com/76584547/131613490-33eaad98-3005-494b-a245-d3e09581ced9.png)


#### 2정규화
---
```
  "부분 종속"
  모든 속성은 반드시 기본키에 종속되어야한다.
  (기본키 일부에만 종속되어서는 안됨)
```
![image](https://user-images.githubusercontent.com/76584547/131613612-bef6dae6-b39d-4094-9766-21c17e960b76.png)

+ 정규화
![image](https://user-images.githubusercontent.com/76584547/131613660-28ec7261-7d76-4f48-b0a7-95677e5072ae.png)


#### 3정규화
---
```
  "이행 종속"
  기본키가 아닌 모든 속성간에는 서로 종속될 수 없다.
```
![image](https://user-images.githubusercontent.com/76584547/131613777-3e0f365f-e50b-4923-a819-eabb8c999c94.png)

+ 정규화
![image](https://user-images.githubusercontent.com/76584547/131613822-f9441c15-6f3f-4e79-91ba-4ff24c9b7e4a.png)


#### 정규화란?
```
  데이터가 꼬이는 것을 막기 위해 테이블을 잘게 나누는 것
```


#### 참고
```
  https://www.youtube.com/watch?v=pMcv0Zhh3J0
```

### 인덱스 장단점
```
  데이터베이스에서 인덱스를 사용하는 이유는 검색성능을 향상시키기 위함입니다.

  하지만 검색성능을 실질적으로 향상시키기 위해서는 해당 쿼리가 index를 사용하는지, 
  카디널리티, Selectivity 같은 요소들이 고려된 인덱스가 생성되어야 합니다.

  일반적인 경우의 장점으로는 빠른 검색 성능을 들 수 있습니다.

  일반적인 경우의 단점으로는 인덱스를 구성하는 비용 즉, 추가, 수정, 삭제 연산시에 인덱스를 형성하기 위한 추가적인 연산이 수행됩니다.
  ex) UPDATE와 DELETE는 기존의 인덱스를 삭제하지 않고 '사용하지 않음' 처리를 해준다고 하였다. 
  만약 어떤 테이블에 UPDATE와 DELETE가 빈번하게 발생된다면 실제 데이터는 10만건이지만 인덱스는 100만 건이 넘어가게 되어, 
  SQL문 처리 시 비대해진 인덱스에 의해 오히려 성능이 떨어지게 될 것이다. 

출처: https://mangkyu.tistory.com/96 [MangKyu's Diary]

  따라서, 인덱스를 생성할 때에는 트레이드 오프 관계에 놓여있는 요소들을 종합적으로 고려하여 생성해야합니다.
  
  참고 : https://github.com/ksundong/backend-interview-question
```

### 인덱스 수직적 탐색, 수평적 탐색
```
  https://blog.naver.com/hgstudy_/222523205918
```

### DB Connection Pool 개수 산정
```
  https://hyuntaeknote.tistory.com/12
```

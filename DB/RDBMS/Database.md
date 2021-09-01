## Database
----
```
  ㅇ 정규화
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

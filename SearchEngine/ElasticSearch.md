### ElasticSearch
-----
```
- 고가용성의 확장 가능한 오픈소스
- 분석 엔진
- 검색 엔진
- nosql처럼 사용 가능
```
+ DB vs ES(Inverted Index : 역 색인)
![image](https://user-images.githubusercontent.com/76584547/120105209-f1bccb80-c192-11eb-9b36-def76346401c.png)
```
- 단어 단위로 잘라서 해당 단어가 어디에 위치하는지 기록
- 재미있다 검색 시 재미 검색 x
- 재미 검색 시 재미있다 검색 x
- 재미있다와 재미를 같은 단어로 검색을 하려면 "형태소 분석"이라는 과정이 필요 (영어는 자동 지원하지만 한글은 지원 x)
- ES에서 공식으로 지원하는 "노리(Nori)"라는 플러그인 이용
```

### Shard (샤드)
---
![image](https://user-images.githubusercontent.com/76584547/120105406-b1aa1880-c193-11eb-823d-a8c72af23c5a.png)

```
- Doc 1~ 100이 있다고 가정 시 서로 다른 4개의 공간으로 저장 (1~25,26~50,51~75,76~100)
- 각각의 샤드는 서로 다른 머신에 올라간다. (노드)
- 노드의 성능은 노드가 가진 cpu,memory,disk의 성능의 영향을 받음
- 샤드의 개념은 ES의 고유의 특징이 아니라 DB에도 있음
```

### Replica (레플리카)
---
![image](https://user-images.githubusercontent.com/76584547/120105512-2b420680-c194-11eb-8f81-c9ab8f6c9c05.png)

```
 - 노드의 복사본을 의미한다.
 - 특정 노드의 문제가 발생했을 때 해당 노드가 포함한 데이터를 다른 노드에도 저장을 하는 것
   (남아있던 복사본 데이터가 있기 때문에 서비스가 계속 가능할 것)
 - Node 1이 죽었다고 가정했을 경우 Node3에 1~50번까지의 레플리카가 저장되었다는 의미
```


### 실습
---
```
 - 구글 클라우드 es-instance 1~4 (4개 생성)
 - sudo yum install -y docker
```

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
 ※ 공통
 - 구글 클라우드 es-instance 1~4 (4개 생성)
 - 1. 도커 설치 : sudo yum install -y docker
 - 2. 도커 시작 : sudo systemctl start docker
 - 3. 도커 실행 권한 : sudo chmod 666 /var/run/docker.sock
 - 4. 가상 메모리 사이즈 키워줌 (ES는 가상메모리를 많이 사용하기 때문에)
      sudo sysctl -w vm.max_map_count=262144
```

### 1번 노드
---
```
  ※ 1번은 네트워크 생성 & 포함 과정이 있음
  1. 네트워크 생성 : docker network create somenetwork 
  2. 네트워크 포함 : 
  2번 명령어---start---- (한줄씩이 아니라 통째로 명 넣기 환경변수)
  ----예시----
  docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 \
  -e "discovery.seed_hosts={2번IP},{3번IP},{4번IP}" \
  -e "node.name=es01" \
  -e "cluster.initial_master_nodes=es01,es02,es03,es04" \
  -e "network.publish_host={1번 IP}" \
  elasticsearch:7.10.1
  ---end----


 docker network create somenetwork
 docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 \
 -e "discovery.seed_hosts=10.146.0.3,10.146.0.4,10.146.0.5" \
 -e "node.name=es01" \
 -e "cluster.initial_master_nodes=es01,es02,es03,es04" \
 -e "network.publish_host=10.146.0.2" \
 elasticsearch:7.10.1
```

### 2번 노드
---
```
docker network create somenetwork
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 \
-e "discovery.seed_hosts=10.146.0.2,10.146.0.4,10.146.0.5" \
-e "node.name=es01" \
-e "cluster.initial_master_nodes=es01,es02,es03,es04" \
-e "network.publish_host=10.146.0.3" \
elasticsearch:7.10.1
```


### 3번 노드
---
```
docker network create somenetwork
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 \
-e "discovery.seed_hosts=10.146.0.2,10.146.0.3,10.146.0.5" \
-e "node.name=es01" \
-e "cluster.initial_master_nodes=es01,es02,es03,es04" \
-e "network.publish_host=10.146.0.4" \
elasticsearch:7.10.1
```


### 4번 노드
---
```
docker network create somenetwork
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 \
-e "discovery.seed_hosts=10.146.0.2,10.146.0.3,10.146.0.4" \
-e "node.name=es01" \
-e "cluster.initial_master_nodes=es01,es02,es03,es04" \
-e "network.publish_host=10.146.0.5" \
elasticsearch:7.10.1
```

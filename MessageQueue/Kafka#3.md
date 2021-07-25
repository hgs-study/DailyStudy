### 멀티 브로커로 카프카 클러스터 구축하기
---
```
  카프카 설치하기 위해 두가지 애플리케이션이 필요
  
  ㅇ 주키퍼 : 카프카 관련 정보를 저장하는 역할
  ㅇ 카프카
```

### 주키퍼
+ 다운로드
```shell
  $ wget https://archive.apache.org/dist/zookeeper/zookeeper-3.4.12/zookeeper-3.4.12.tar.gz
```

+ zookeeper 압축 풀기
```shell
  $ tar xvf zookeeper-3.4.12.tar.gz
```

+ 주키퍼 앙상블 구축
  + 주키퍼 설정
  + 이제 zookeeper의 configuration을 설정해야합니다. zookeeper폴더내부의 conf폴더에 zoo.cfg파일을 생성하여 아래와 같이 configuration을 넣어줍니다.
```shell
tickTime=2000
dataDir=/var/lib/zookeeper
clientPort=2181
initLimit=20
syncLimit=5
server.1=test-broker01:2888:3888
server.2=test-broker02:2888:3888
server.3=test-broker03:2888:3888
```
![image](https://user-images.githubusercontent.com/76584547/126800784-60c4f8b9-774d-4530-80e2-f09489e6422f.png)


+ 호스트 네임 수정
```shell
$ vim /etc/hosts
  
[test-broker01 인스턴스]
0.0.0.0 test-broker01
3.37.104.87 test-broker02
3.37.93.184 test-broker03
  
[test-broker02 인스턴스]
0.0.0.0 test-broker02
3.35.30.214 test-broker01
3.37.93.184 test-broker03
  
[test-broker03 인스턴스]
0.0.0.0 test-broker03
3.35.30.214 test-broker01
3.37.104.87 test-broker02
```
![image](https://user-images.githubusercontent.com/76584547/126800736-ffcaaf98-8701-4932-b0bd-5760bd032ff1.png)


+ myid 설정
  + 이제 zookeeper 앙상블을 만들기 위해 각 zookeeper마다 myid라는 파일을 만들어줘야합니다. myid의 위치는 /var/lib/zookeeper/myid 이고, 해당 파일에는 숫자를 하나 넣으면 됩니다. test-broker01은 1, test-broker02는 2, test-broker03은 3 으로 지정합니다.
```shell
  $ sudo vim /var/lib/zookeeper/myid
  $ cat /var/lib/zookeeper/myid // test-broker01 => 1
  $ cat /var/lib/zookeeper/myid // test-broker02 => 2
  $ cat /var/lib/zookeeper/myid // test-broker03 => 3
```



+ 주키퍼를 서로 연동하기 위해서 방화벽 설정
```
  주키퍼 : 2181포트 , 2888:3888포트 사용
    - 2181 2888 3888 3개 포트를 anywhere 조건으로 방화벽을 엶
    - 카프카 포트를 위해 9092 포트도 엶
```

![image](https://user-images.githubusercontent.com/76584547/126801069-cbe6cb04-e53b-4124-92b1-859ccc34f3ec.png)


+ 각 서버에서 주키퍼 실행
```shell
  $ ./bin/zkServer.sh start (주키퍼 압축 푼 경로에서)
```
+ 자바 설치 (open jdk 1.8.0)
```shell
  sudo yum install -y java-1.8.0-openjdk-devel.x86_64
```

+ 각 주키퍼 연결 (주키퍼 서버 - 다른 인스턴스)
```shell
  $ ./zkCli.sh -server 인스턴스Ip:2181
```

### 카프카
+ 다운로드
```
  $ wget https://archive.apache.org/dist/kafka/2.1.0/kafka_2.11-2.1.0.tgz
```

+ zookeeper 압축 풀기
```shell
  $ tar xvf kafka_2.11-2.1.0.tgz
```

+ broker.id 설정, zookeeper에 대한 설정과 listener설정 (카프카 실행을 위해)
  + 대상 파일은 kafka 폴더 내부에 config/server.properties 입니다.
  + 참고로 zookeeper 설정시 마지막에 /test 와 같이 route를 넣는 것을 추천드립니다. 이렇게 넣을 경우 zookeeper의 root node가 아닌 child node에 카프카정보를 저장하게 되므로 유지보수에 이득이 있습니다.
  +  listener 설정시 PLAINTEXT는 Un-authenticated, non-encrypted channel 을 뜻합니다. 상세한 내용은 아래 2개 링크에서 확인하실 수 있습니다.
```shell
  $  카프카 압축 푼 폴더/config/server.properties
  $ vim server.properties
  
  - broker.id를 각 서버별로 다른 숫자로 설정 (0 , 1, 2)
  - zookeeper.connect=test-broker01:2181,test-broker02:2181,test-broker03/test
  - listeners=PLAINTEXT://:9092
  - advertised.listeners=PLAINTEXT://test-broker01:9092
```

![image](https://user-images.githubusercontent.com/76584547/126811748-25a8329a-7d1a-4b56-b935-56ed6b6129e4.png)


![image](https://user-images.githubusercontent.com/76584547/126812006-51214130-dca0-4704-8d40-e3c23fea59b6.png)


+ 카프카 실행
```shell
  $ ./kafka-server-start.sh ../config/server.properties (카프카/bin에서)
```

+ 메모리 문제
```shell
  Native memory allocation (mmap) failed to map 1073741824 bytes for committing reserved memory.
```

+ 원인
```
  kafka가 활성화되면 기본적으로 1GB의 메모리를 할당해야 합니다. 무료 서버 성능 제한으로 인해 메모리 할당 예외가 발생합니다.
```

+ 해결
  + kafka-server-start.sh파일 수정
![image](https://user-images.githubusercontent.com/76584547/126813109-ea7406f7-c2b6-4160-93e1-13a928e1cf15.png)


+ etc/hosts 변경 (주키퍼 서버)
```shell
$ sudo /etc/hosts

3.35.30.214 test-broker01
3.37.104.87 test-broker02
3.37.93.184 test-broker03
```

+ 테스트 토픽(test_log) 생성 (주키퍼 서버)
```
  $ ./kafka-topics.sh --create --zookeeper test-broker01:2181,test-broker02:2181,test-broker03/test --replication-factor 1 --partitions 1 --topic test_log
```

![image](https://user-images.githubusercontent.com/76584547/126889992-1ca51e89-fb37-413d-8751-4d0b15c6154c.png)


+ 토픽(test_log)에 console-producer로 데이터 넣기 (주키퍼 서버)
```shell
  $ ./kafka-console-producer.sh --broker-list 3.35.30.214:9092,3.37.104.87:9092,3.37.93.184:9092 --topic test_log
```
![image](https://user-images.githubusercontent.com/76584547/126890059-74abcc7a-f260-4cb6-b075-790d158df35f.png)

+ 토픽에 producer로 데이터를 넣는 동시에 consumer로 확인하기 (주키퍼 서버)
```shell
  $ ./kafka-console-consumer.sh --bootstrap-server 3.35.30.214:9092,3.37.104.87:9092,3.37.93.184:9092 --topic test_log --from-beginning
```

+ 프로듀서에서 메세지 전달

![image](https://user-images.githubusercontent.com/76584547/126890139-c580e377-8d1f-4c06-a1e7-a12cca1207c4.png)


+ 컨슈머에서 메세지 받음

![image](https://user-images.githubusercontent.com/76584547/126890145-793c46db-cbed-4214-866b-7bc1599210a4.png)


+ 라운드 로빈 방식으로 offset 증가하는 걸 확인할 수 있다
  + test-broker01
  ![image](https://user-images.githubusercontent.com/76584547/126890243-aa50458f-4194-4b52-bfe0-1c1fe7cfc3c9.png)

  + test-broker02
  ![image](https://user-images.githubusercontent.com/76584547/126890254-b6e62737-df68-4a63-beea-6b3974e1fad1.png)
  
  + test-broker03
  ![image](https://user-images.githubusercontent.com/76584547/126890258-f2cb4a4e-0929-4de0-b0dc-ef2db8ca3ca8.png)





참고 : https://www.youtube.com/watch?v=Qr0HVvtMFhg&t=111s
참고 : https://blog.voidmainvoid.net/325

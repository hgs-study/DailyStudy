### 설치
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
server.2=test-broker03:2888:3888
server.3=test-broker02:2888:3888
```

+ 호스트 네임 수정
```shell
  $ vim /etc/hosts
  
  ㅇ test-broker01 인스턴스
  127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
  ::1         localhost6 localhost6.localdomain6

  0.0.0.0 test-broker01
  52.78.74.23 test-broker02
  52.78.229.105 test-broker03
  
  ㅇ test-broker02 인스턴스
  127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
  ::1         localhost6 localhost6.localdomain6

  0.0.0.0 test-broker02
  3.35.131.185 test-broker01
  52.78.229.105 test-broker03
  
  ㅇ test-broker03 인스턴스
  127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
  ::1         localhost6 localhost6.localdomain6

  0.0.0.0 test-broker03
  3.35.131.185 test-broker01
  52.78.74.23 test-broker02
```

참고 : https://www.youtube.com/watch?v=Qr0HVvtMFhg&t=111s
참고 : https://blog.voidmainvoid.net/325

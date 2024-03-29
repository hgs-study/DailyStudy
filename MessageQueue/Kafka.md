## Kafka

### Kafka 란?
---
```
  - 데이터를 전송하는 소스 애플리케이션과 데이터를 받는 타겟 애플리케이션 중간에서 데이터를 전송해주는 큐 역할을 한다.
  - 카프카는 고가용성으로서 서버가 이슈가 생기거나, 갑작스럽게 랙(전원)이 내려가더라도 데이터를 손실 없이 복구할 수 있다.
  - 애플리케이션 사이에서의 커플링을 낮출 수 있다.
  - 카프카는 링크드인에서 개발했으며 현재는 오픈소스로 사용되어지고 있다.
```

![image](https://user-images.githubusercontent.com/76584547/124613926-b35fae00-deae-11eb-92b9-c359226eb8ee.png)


### 토픽이란?
---
```
  - 카프카 내에서 데이터가 들어가는 공간 (큐)
```

![image](https://user-images.githubusercontent.com/76584547/124615205-00904f80-deb0-11eb-9da4-32755e860f11.png)

<br/>

+ 하나의 토픽(clikc_log)는 여러개의 파티션을 가질 수 있으며 데이터(record)가 쌓임
```
  - 데이터(record)는 내부에서 하나씩 쌓이며, 오래된 순 (0 -> 1 -> 2 -> 3 ...)으로 컨슈머에 전달된다.
  - 6번까지 데이터가 전달된 후 또 다른 데이터가 들어올 때까지 컨슈머는 기다린다.
  - 컨슈머가 record들을 가져가도 데이터는 삭제되지 않고 파티션에 그대로 남게된다.
```
![image](https://user-images.githubusercontent.com/76584547/124616233-dd19d480-deb0-11eb-8cc5-6f0bca9eae4c.png)

<br/>

+ 그대로 남게된 record들은 새로운 컨슈머가 붙었을 때 다시 0번부터 가져가서 사용할 수 있다.
```
  - 다만 컨슈머 그룹이 달라야하고
  - auto.offset.reset = earliest 설정이 되어있어야한다.
  - 동일 데이터를 2번 사용할 수 있다 (카프카를 사용하는 아주 중요한 이유)
    => 클릭로그를 es에 저장해야하고, 하둡에 저장해야하기 때문에
```

![image](https://user-images.githubusercontent.com/76584547/124616766-5285a500-deb1-11eb-80f2-af5142dd225c.png)

<br/>

+ record 적재 (파티션이 여러개일 경우)
```
  1. 키가 null이고, 기본 파티셔너 사용할 경우
    => 라운드 로빈으로 할당
  2. 키가 있고, 기본 파티셔너 사용할 경우
    => 키의 해시(hash)값을 구하고, 특정 파티션에 할당
```

![image](https://user-images.githubusercontent.com/76584547/124617338-d17add80-deb1-11eb-8152-2777c58a1fa2.png)
=> 키가 없을 경우 '7'은 파티션#1에 할당된다.


<br/>

+ 파티션이 여러개일 경우 고민해봐야한다.
```
  - 파티션을 늘릴 수는 있지만 줄일 수는 없다.
  - 파티션을 늘리는 이유?
    => 파티션을 늘리면 컨슈머의 개수를 늘려서 데이터 처리를 분산시킬 수 있기 떄문에
```

<br/>

+ record는 언제 삭제되는가?
```
  - log.retention.ms : 최대 record 보존시간 설정 가능
  - log.retention.byte : 최대 record 보존 크기(byte) 설정 가능
```


### 브로커(broker)란?
---
```
  - 카프카가 설치되어 있는 서버 단위
  - 보통 3개 이상의 브로커로 구성하여 사용하는 것을 권장
```

+ 만약 파티션이 1개이고, 레플리케이션이 1인 토픽이 존재하고 브로커가 3대라면 브로커 3대 중 1대에 해당 토픽의 정보(데이터)가 저장된다.
![image](https://user-images.githubusercontent.com/76584547/125185339-4c037e80-e25f-11eb-8aec-e762a2a38446.png)


### 레플리케이션(replication)이란?
---
```
  - 파티션의 복제를 뜻한다
  - 파티션의 고가용성을 위해 사용된다.
  - 하나의 브로커가 장애가 생기더라도 다른 복제본 파티션(레플리카)이 있으므로 사용 가능하다.
  - 나머지 하나 남은 복제본 파티션이 Leader partition 역할을 승계하는 것
```

+ 만약 레플리케이션이 1이라면, 파티션은 1개만 존재한다는 것이고
+ 만약 레플리케이션이 2라면, 파티션은 원본 1개와 복제본 1개로 존재한다는 것
+ 만약 레플리케이션이 3이라면, 파티션은 원본 1개와 복제본 2개로 존재한다는 것
+ 원본 파티션은 Leader partition이라고 부른다.
+ 복제 파티션은 Follower partition 이라고 부른다.

![image](https://user-images.githubusercontent.com/76584547/125186078-3c863480-e263-11eb-8a88-6f828fa0be70.png)

+ 브로커 개수에 따라 replication 개수가 제한된다.
+ 브로커 개수가 3이면 레플리케이션은 4가 될 수가 없다.
+ 레플리케이션이 많아지면 그만큼 브로커 리소스량도 많아진다 
  + 하나에 1TB라고 가정했을 경우, 브로커 개수만큼 1TB x 개수가 된다. 
  + 카프카에 들어오는 데이터량과 retention date 즉, 저장시간을 잘 생각해서 레플리케이션 개수를 정하는 것이 좋다.
  ※ 3개 이상의 브로커를 사용할 때 레플리케이션을 3으로 하는 것

<br/>

+ Leader partition
  + Kafka 프로듀서가 토픽으로 데이터를 전달할 때 전달받는 주체가 바로 Leader patition 
  + 프로듀서에는 ack라는 상세 옵션이 있다.
  + ack를 고가용성을 유지할 수 있는데, 이 옵션은 partition의 replication과 관련이 있다.
  ```
    ㅇ ack는 0,1,all (3개 중 1개 선택 가능)
    
    ㅇ 0일 경우, Leader patition에게 데이터를 전송하고 응답값을 받지 않는다.
      - 응답값을 받지 않기 때문에 Leader partition에 데이터가 정상적으로 전송됐는지 알 수 없다.
      - 또한 나머지 partition에 정상적으로 복제되었는지 알 수 없고, 보장할 수 없다. 
      - 속도는 ↑, 하지만 데이터 유실 가능성 있다.
      
    ㅇ 1일 경우, Leader patition에게 데이터를 전송하고 응답값을 받는다.
      - 다만 나머지 파티션이 복제되었는지 알 수 없다.
      - Leader partition이 데이터를 받자마자 장애가 나면 나머지 partition에 데이터가 미처 전송되지 못한 상태이기 때문에
      - 데이터 유실 가능성이 있다.

    ㅇ all일 경우, Leader patition에게 데이터를 전송하고 응답값을 받고, followr partition에 복제가 잘 이루어졌는지 응답값 받음
      - Leader partition에 데이터를 보낸 후, 나머지 partition에도 복제가 이루어졌는지 확인 절차를 거침
      - 속도는 ↓, 데이터 유실은 없다.
  ```
  
    


### ISR(In Sync Replica)
----
```
  - 만약 리더가 있는 브로커가 장애가 난다면 팔로워는 새로운 리더가 될 수 있다. 
```
+ Leader partition, Follower partition을 합쳐서 ISR이라고 볼 수 있다.
![image](https://user-images.githubusercontent.com/76584547/125186230-12814200-e264-11eb-81dd-9475b7a55943.png)



### 참고 링크 
---
- https://zeroco.tistory.com/105?category=1005456
- https://zeroco.tistory.com/115

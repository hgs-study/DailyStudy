## HA

- 실제 서비스에 레디스를 사용하려면 HA는 필수
- Redis는 Sentinel이라는 데몬을 사용

<br>

## 마스터 장애 복구

1. 마스터의 장애를 정확히 판별한다.
2. 슬레이브를 마스터로 승격시킨다.
3. 해당 작업 내용을 클라이언트에게 통지한다.
- 3번의 작업일 경우 Sentinel에서는 이미 장애가 발생한 마스터에 접속된 클라이언트를 알 수 없으므로 해당 알림을 원하는 클라이언트는 `Redis Pub/Sub`으로 Sentinel에 등록해야한다

<br>

## Sentinel 장애 판별

- 기본적으로 `Ping` 명령어를 사용
- 하지만 응답이 없다고 해서 바로 해당 서버가 장애라고 판단하여 마스터를 변경하지 않음
- Sentinel은 `Sdown`과 `Odown` 이라는 두가지 상태로 장애를 인식
- `Sdown` : Sentinel 하나가 해당 서버가 장애라고 인식하는 주관적인 장애 상태
- `Odown` : 여러대의 Sentinel이 해당 서버가 장애라고 인식하는 객관적인 장애 상태
- Sentinel은 Failover를 위한 정족수 설정이 있는데, 이 값 이상의 Sentinel 서버가 장애라고 판단하면 해당 서버는 ODOWN 상태가 된다.

### 정족수 설정시

- 만약 정족수를 3이라고 설정했는데, Sentinel이 2대라면 장애가 발생하더라도 마스터로 승격할 수가 없다.
- 보통 Sentinel을 `홀수로 두고 그 과반수를 정족수로 설정`하는 것이 좋다.

<br>

## 마스터로 승격할 슬레이브는 어떻게 선택할까?

### 조건
- Sdown, Odown, Disconnection 된 슬레이브는 제외
- 각종 벨리데이션 타임보다 작으면 제외
- 남은 후보들 중에서 slave_priority가 높은 슬레이브가 우선적으로 선택되고, slave_priority가 같으면 runid를 비교해서 가장 큰 값의 마스터로 선택 (slave_priority가 0이면 제외)
- 절대 승격되지 않았으면 하는 슬레이브가 필요하다면? (백업서버가 필요하거나, 절대 서비스에 직접적으로 투입되지 않기를 바랄때)
    - ⇒ `slave_priority 0`으로 설정하면 절대 마스터로 승격하지 않는다

### 플로우

1. **wait_start** : Failover가 시작되었다.   Epoch를 증가시키고 장애조치를 주관할 센티널을 선정한다.
2. **select_slave** : 새로운 마스터가 될 복제를 선정한다.
3. **send_slaveof_noone** : 선정된 복제에게 SLAVEOF NO ONE 명령을 실행한다.
4. **wait_promotion** : SLAVEOF NO ONE 명령이 완료되기를 기다리는 상태.
5. **reconf_slaves**
 : 복제들에게 새 마스터를 바라보도록 SLAVEOF New-IP New-Port 명령을 실행한 상태.   SLAVEOF 명령은 복제마다 실행하며, 한 복제가 완료되면 다음 복제에 명령을 내린다.   slave-reconf-sent에서 done까지 복제는 새 마스터로부터 전체 데이터를 받는(Full resync) 과정이고, 소요 시간은 데이터 량에 따라 다르다.  slave-reconf-sent, inprog, done는 복제 개수만큼 로그에 남는다.
6. **update_config** : 센티널이 가지고 있는 정보를 새 마스터와 복제로 갱신하고 새 마스터를 모니터링하기 시작한다.   
- 참고 : [http://redisgate.jp/redis/sentinel/sentinel_masters.php](http://redisgate.jp/redis/sentinel/sentinel_masters.php)

<br>

## 설정

- sentinel.conf에 하나의 클러스터(마스터/슬레이브 쌍)을 위한 설정

```bash
## sentinel.conf

sentinel monitor resque 127.0.0.1 2001 2
sentinel down-after-milliseconds resque 3000
sentinel failover-timeout resque 900000
sentinel can-failover resque yes
sentinel parallel-syncs resque 1
```

1. 모니터링할 마스터 서버와 주소와 해당 클러스터의 이름, 정족수로 구성

```bash
sentinel monitor resque 127.0.0.1 2001 2
sentinel monitor <클러스터명> <마스터 IP> <마스터 Port> <정족수>
```

2. 다운으로 인식하는 시간을 설정

```bash
sentinel down-after-milliseconds resque 3000
sentinel down-after-milliseconds <클러스터명> <시간 miliseconds>
```

3. Failover시에 사용되는 timeout을 설정

```bash
sentinel failover-timeout resque 900000
sentinel failover-timeout <클러스터명> <시간 miliseconds>
```

4. Failover할 것인지 여부이며, yes로 해야 마스터가 장애 발생 시 Sentinel이 Failover 진행

```bash
sentinel can-failover resque yes
sentinel can-failover <클러스터명> <시간 miliseconds>
```

5. 새로운 마스터 승격 후에 몇개의 슬레이브가 싱크를 해야 실제로 클라이언트에게 알려줄 것인지 설정

```bash
sentinel parallel-syncs resque 1
sentinel parallel-syncs <클러스터명> <sync할 slave 숫자>
```

<br>

## Sentinel은 다른 노드를 어떻게 발견할까?

```bash
sentinel monitor resque 127.0.0.1 2001 2
```

- 위 명령처럼 마스터의 IP와 Port밖에 적지 않는데 어떻게 마스터와 슬레이브, Sentinel의 주소까지 알 수 있을까?
- `마스터/슬레이브` : info 명령으로 확인. 슬레이브라면 마스터의 주소가 있고, 마스터라면 슬레이브의 주소를 info에서 확인해서 목록을 가져올 수 있다
- `다른 센티널 노드` : Sentinel은 현재 Redis 마스터 노드에 “SENTINEL_HELLO_CHANNEL” 이라는 Pub 채널을 만드는데, 새로 접속한 Sentinel은 해당 채널에 hello 메시지를 전달하며 이를 통해 서로의 존재를 알 수 있다.

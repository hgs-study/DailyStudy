## Redis가 메모리를 두배로 사용하는 문제

- RDB를 저장하는 Persistent 기능으로, fork를 사용하기 때문에
- 이전엔 OS가 자식 프로세스를 생성하려면 부모 프로세스의 메모리를 모두 자식 프로세스에게 복사해야했다.
- 시간이 지나 OS가 발전하면서 COW(Copy On Write) 라는 기술 개발하여, fork 후 자식 프로세스와 부모 프로세스의 메모리에서 실제로 변경이 일어난 부분만 차후에 복사
- 하지만 Redis와 같은 솔루션을 사용하는 곳은 대부분 Write가 많으므로 예전의 경우와 마찬가지로 메모리를 두 배로 사용하는 경우가 생긴다.

## Read는 가능한데 Write만 실패하는 경우

- Redis 서버는 동작하지 않는데, 정기적인 Heartbeat체크에는 이상이 없다고 나오는 황당한 상황을 경험한 적이 있을 것이다.
- 이러한 문제는 RDB 저장이 실패할 때, 기본 설정상 Write 관련 명령이 동작하지 않기 때문이다.
- 즉, Redis의 기본 설정상 RDB 저장이 실패하면 해당 장비에 뭔가 이상이 있다고 생각하여 Write 명령을 더는 처리하지 않으며, 데이터가 변경되지 않도록 관리한다.
- 일반적으로 Heartbeat 체크는 읽기 관련 명령을 이용하여 검사하기 때문에 그런 상태에 있다는 것을 확인하지 못하고 서비스 장에가 발생한 것으로 인식하기 때문이다
- RDB 생성 실패 원인
    1. RDB를 저장할 수 있을 정도의 디스크 여유 공간이 없는 경우
    2. 실제 디스크가 고장난 경우
    3. 메모리 부족으로 인해서 자식 프로세스를 생성하지 못한 경우
    4. 누군가 강제적으로 자식 프로세스를 종료시킨 경우
    5. 그 외 기타 등등

## 해결방법

인지) 해당 상황이 맞는 지를 확인

```bash
1.
$redis 127.0.0.1:6379 > set a 123
(error) MISCONF Redis is configured to save RDB snapshots, but is currently 
not able to persist on disk. Commands ~

---
2.
$redis 127.0.0.1:6379 > info
~
rdb_last_bgsave_status:ok
~
```

- 1번은 Write 명령어를 실행할 경우, RDB snapshot을 저장하지 못했다는 오류가 뜨고
- 2번은 redis info 명령어를 확인해보면 rdb_last_bgsave_status 이 ok인 것을 확인할 수 있다.

해결) 이 상황에서 Write 명령을 허용 가능하게 결정

```bash
1.
$redis 127.0.0.1:6379 > config set stop-writes-on-bgsave-error no
OK

----
2. #redis.conf에 미리 등록
stop-writes-on-bgsave-error no    # default yes
```

- 1번은 이미 운영중인 레디스 서버에서 변경할 수 있다
- 2번은 2.6.12에서 지원하지 않다가 Redis 2.6.13 버전에서 추가됐다

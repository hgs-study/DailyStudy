### 설치
----
```
- docker run -d --hostname my-rabbit --name some-rabbit -p 5672:5672 -p 15672:15672 rabbitmq:3-management
- 5672:5672 : RabbitMQ와 메시지를 주고 받는 포트
- 15672:15672 : 모니터링 툴을 사용하기 위해 열어야하는 포트
- http://localhost:15672/ 접속 (아이디 / 비밀번호 : guest)
```

### 세팅
----
```
- [Queues] - [Add a new queue] - [NAME] 지정 : CREATE_POST_QUEUE
- [CREATE_POST_QUEUE] - [Publish message] - [Payload] :  ([Publish message] 메시지 만듦)
  {
     "content" : "my post"
  }
- [CREATE_POST_QUEUE] - [Get message] : 메시지 가져옴 (requeue true, requeue false)
  - requeue true 시 Get message 이후 다시 Queue에 들어감
  - requeue false 시 Get message 이후 다시 Queue에 들어가지 않음
```

### Producer
----
```
- 메시지를 큐로 집어넣는 역할
```

### Consumer
----
```
- 메시지를 큐에서 꺼내오는 역할
```

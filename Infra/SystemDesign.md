## System Design
---
```
  ㅇ 처리율 제한 장치
```

### 처리율 제한 장치
----
```
  - 클라이언트 또는 서비스가 보내는 트래픽의 처리율(rate)을 제어하기 위한 장치다.
  - HTTP로 예를 들면 특정 기간 내에 전송되는 클라이언트의 요청 횟수를 제한한다.
```
+ API Gateway를 사용한다.

#### API Gateway
---
```
  1. 처리율 제한
  2. SSL 종단
  3. 사용자 인증
  4. IP 허용 목록관리
```
![image](https://user-images.githubusercontent.com/76584547/130916803-006debea-6070-4826-abe9-0db6ae921890.png)

+ API Gateway를 API 서버 앞에 두어 해당 기간 안에 특정 요청횟수만큼 요청을하면 429 : Too many request를 응답한다.
+ 처리율 제한 알고리즘
  + 토큰 버킷
  + 누출 버킷
  + 고정 윈도 카운터
  + 이동 윈도 로그
  + 이동 윈도 카운터

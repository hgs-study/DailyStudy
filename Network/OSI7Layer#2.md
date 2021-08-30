## OSI 7 Layer#2
---
```
  국제표준기구 iso가 발표한 네트워크 모델
```
+ 네트워크 발표한 이전 상황
```
  ㅇ A라는 업체의 네트워크 장비를 통한 네트워크는 잘 통신이 된다.
  ㅇ 하지만 A와 B라는 업체의 네트워크를 같이 통신하면 장비와 규격이 다르기 때문에 통신이 안된다.
  ㅇ 이 때문에 1984년 osi 모델이라는 것을 발표한다.
```
 
+ 목록
```
  ㅇ Application Layer
  ㅇ Presentation Layer
  ㅇ Session Layer
  ㅇ Transport Layer
  ㅇ Network Layer
  ㅇ Data-Link Layer
  ㅇ Physical Layer
```

### Application Layer
---
```
  ㅇ 7계층
  ㅇ 응용 프로세스를 직접 사용하여 직접적인 응용 서비스를 사용
  ㅇ 말이 어렵지만 우리가 사용하고 있는 HTTP, FTP, SMTP, Telnet 같은 프로토콜
```
![image](https://user-images.githubusercontent.com/76584547/131274750-251fcf3f-3dfa-4cdb-8d62-e0b153975be2.png)


### Presentation Layer
---
```
  ㅇ 6계층
  ㅇ 데이턴의 변환, 압축, 암호화가 이루어진다.
  ㅇ 서로 다른 통신 기기 간에 다른 인코딩을 사용할 수도 있기 때문에 해당 계층에서 데이터 변환이 이루어진다.
```
![image](https://user-images.githubusercontent.com/76584547/131274859-713c23f3-9670-4c10-a828-30231254c5b6.png)


### Session Layer
---
```
  ㅇ 5계층
  ㅇ 세션을 열고 닫고를 제공하는 메커니즘 계층
  ㅇ "세션 복구" 지원
  ㅇ 세션 복구 : "체크포인트"라는 것을 통해 동기화 시켜준다.
  ㅇ ex) 컴퓨터 A에서 B로 100MB의 데이터를 전송한다고 했을 때, 체크포인트를 5MB마다 설정한 가정하고
         48MB의 데이터를 전송하는 도중에 연결이 끊기게 되었는데, 체크포인트 덕분에 다시 45MB부터 세션을 재개할 수 있다.
```
![image](https://user-images.githubusercontent.com/76584547/131275588-c3a4773e-4750-43f2-99ac-61bbaa5ea640.png)



### Transport Layer
---
```
  ㅇ 4계층
  ㅇ 서로 다른 두 네트워크간의 전송을 담당하는 계층
  ㅇ 세그멘테이션, 흐름제어, 오류제어 등을 제공
  ㅇ 세그멘테이션 : 상위 계층 데이터를 받아서 세그먼트라는 단위로 나누는 것을 의미
  ㅇ 세그멘테이션 하는 이유? 
       => 한 사용자가 100MB 비디오를 전송 받는다고 가정했을 때 세그멘테이션하지 않을 경우 전부 로딩돼야 볼 수 있다.
       => 중간에 연결이 끊겼을 경우 세그멘테이션하지 않았더라면 큰 데이터가 날라갔을 것
  ㅇ 흐름제어 : 서로 다른 데이터 전송량이 다른 기기에서 
       => A : 50Mbps, B : 10Mbps 통신할 경우,
       => B가 A에게 전송량을 낮춰달라고 요구하고 전송량을 낮추는 것
  ㅇ 오류제어 : 제가 보낸 데이터가 오류가 없는지 만약 오류가 있다면 다시 보내주는 것
```
+ 세그멘테이션
![image](https://user-images.githubusercontent.com/76584547/131275574-b7662a17-3235-4d10-9fb7-f5cce3560f2a.png)

+ 흐름제어
![image](https://user-images.githubusercontent.com/76584547/131276340-e56b520c-fcfc-47bb-86d7-f7b2f5ebdb99.png)


### Network Layer
---
```
  ㅇ 3계층
  ㅇ IP, 라우터가 속한 계층
  ㅇ 데이터의 전송 담당
  ㅇ 호스트에 IP번호를 부여하고 해당 도착지 IP까지 최적의 경로를 찾아주는 것 => 라우팅
```
![image](https://user-images.githubusercontent.com/76584547/131276668-24e7aa41-e5f3-4034-9751-cc8777ec0263.png)


### Data Link Layer
---
```
  ㅇ 2계층
  ㅇ 네트워크 계층과 상당히 비슷 <=> 
  ㅇ 차이점 
    => 네트워크 계층 : 서로 다른 두 네트워크 간의 전송 담당
    => 데이터링크 계층 : 동일한 네트워크 간 전송 담당
  ㅇ 오류제어 
```
+ 10개의 프레임에서 2개의 프레임이 오류가 났을 경우 그냥 버려버린다
![image](https://user-images.githubusercontent.com/76584547/131276960-43bd815e-ce4c-4af3-b6be-9070aae23934.png)

+ 트랜스포트 계층(4계층)에선 해당 데이터가 없으면 다시 보내준다.
![image](https://user-images.githubusercontent.com/76584547/131277018-6b55c52e-d1a6-4bec-8194-117ff29369ac.png)


### Physical Layer
---
```
  ㅇ 1계층
  ㅇ "0101110" 같은 비트 단위들을 전기 신호로 변환해주고 전송해주는 역할
```
![image](https://user-images.githubusercontent.com/76584547/131280300-be79a256-3543-45c8-9825-65ffffa0194b.png)

 
### 참고
---
```
  https://www.youtube.com/watch?v=Fl_PSiIwtEo&t=250s
```

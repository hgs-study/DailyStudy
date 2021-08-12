## OSI 7 Layer
---
```
  ㅇ Psysical Layer
  ㅇ Data-Link Layer
```

### Psysical Layer
---
+ A 컴퓨터, B컴퓨터 : 두 컴퓨터가 통신할 경우!
  + A컴퓨터의 0과 1의 나열을 아날로그 신호로 바꾸어 전선으로 흘려 보내고 (encoding)
  + B컴퓨터에 아날로그 신호가 들어오면 0과 1의 나열로 해석하여 (decoding)
  + 물리적으로 연결된 두 대의 컴퓨터가 0과 1의 나열을 주고 받을 수 있게 해주는 모듈 (module)

<br/>

![image](https://user-images.githubusercontent.com/76584547/128634323-53ba745a-c5ac-4dbd-b684-25e2316350db.png)


![image](https://user-images.githubusercontent.com/76584547/128634238-7722cd2a-6c72-4c7e-b1a4-f30213b3c927.png)


#### Psycial Layer 기술은 어디에 구현되어 있을까?
---
  + PHY 칩
  + 사실 1계층 모듈은 하드웨어적으로 구현되어 있다.


### Data-Link Layer 
---

#### 학습 전
---
+ 여러 대의 컴퓨터가 통신을 하려면?
  + A와 B는 전선으로 연결되어 있으므로 컴퓨터로 데이터를 주고 받을 수 있다.
  + A는 C한테도 데이터를 보내고 싶어졌다.
  + 하지만.. D,E,F,G 등 컴퓨터에 또 연결하고 싶을 경우 계속 전선을 늘려줘야한다..
  + 그렇기 때문에 전선을 하나를 가지고 여러대의 컴퓨터와 통신할 방법을 모색해야한다.

+ 해결
  + A,B,C,D,E 사이에 스위치를 두고 A가 원하는 곳으로 데이터를 전송한다.

![image](https://user-images.githubusercontent.com/76584547/129146812-c854a3ca-c8b7-4a94-93e3-973699ec8d8d.png)

+ 서로 다른 네트워크 
  + 하나의 스위치와 컴퓨터가 구성한 것을 "네트워크"라고하고, 이것으 "인트라넷"이라고 한다.
  + 아래 그림에서는 네트워크가 2개 있는 것
  + 이 상황에서 "예림"이가 "혜림"이한테 데이터를 보내고 싶어한다 (서로 다른 네트워크)
![image](https://user-images.githubusercontent.com/76584547/129147150-3617be1f-0155-45a6-9ee2-0f079678afe6.png)

+ 라우터
  + 스위치와 스위치를 연결해서 서로 다른 네트워크에 속한 컴퓨터끼리 통신이 가능하게 해주는 장비
  + 라우터로 스위치를 연결했기 때문에 예림이와 혜림이는 통신이 가능하다.
![image](https://user-images.githubusercontent.com/76584547/129147407-533d5420-4b5a-430b-94f2-a1b35b228b6d.png)

+ 공유기
  + 이 때, 초록색 기계는 우리가 잘 알고 있는 "공유기"에 해당된다. 
![image](https://user-images.githubusercontent.com/76584547/129148032-65737a8c-9e64-446b-b01d-5b4921440c56.png)

+ 더 많은 네트워크를 연결해보자
  + 라우터와 라우터를 라우터로 연결할 수 있다.
  + 이런식으로 "계층형"으로 연결해서 전 세계 컴퓨터들을 연결한 것을 "인터넷"이라고 한다.
![image](https://user-images.githubusercontent.com/76584547/129148157-78c4bd84-66ed-4b90-a0c0-f625531e84e4.png)

 
#### Data-Link Layer 
---
+ 한 번에 들어오는 데이터를 끊어 읽어야할 필요가 생겼다
![image](https://user-images.githubusercontent.com/76584547/129148886-20df78cc-b77a-4e75-99ff-7c8d6971596f.png)

+ Framing(프레이밍) : 시작은 1111, 끝은 0000을 붙여서 그 안에 데이터를 읽으며, 데이터를 끊어 읽는다.
![image](https://user-images.githubusercontent.com/76584547/129148948-9b777019-f69f-45c7-93e5-dade5aed003f.png)

+ 즉 Data-Link Layer란?
  + 같은 네트워크에 있는 여러 대의 컴퓨터들이 데이터를 죽조 받기 위해서 필요한 모듈
  + Framing은 Data-link Layer에 속하는 작업들 중 하나입니다.
  ![image](https://user-images.githubusercontent.com/76584547/129148820-f6fb990e-cda0-4469-bed9-13d5fbf29285.png)

+ 1계층과 2계층을 합한 통신
```
  1. 2계층 데이터를 인코더에 넣는다.
  2. 인코딩된 데이터는 프레이밍(앞 1111, 끝 0000)처리돼서 나온다.
  3. 1계층 인코더로 아날로그 시그널로 변경한다.
  4. 받는 쪽 컴퓨터에서 아날로그 시그널을 프레이밍된 데이터를 받는다.
  5. 2계층 디코더로 데이터를 받는다.
```
![image](https://user-images.githubusercontent.com/76584547/129149334-4cadd336-730a-4678-a275-490a695bb570.png)

#### Data-link Layer 기술은 어디에 구현되어 있을까?
---
+ 랜카드
+ 2계층 모듈도 1계층 모듈처럼 하드웨어 모듈로 구성되어 있다.

### 참고 
---
우아한 Tech : https://www.youtube.com/watch?v=1pfTxp25MA8
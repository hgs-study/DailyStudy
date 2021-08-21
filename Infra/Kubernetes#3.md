## 쿠버네티스 인사이드

### 쿠버네티스 구성요소 확인 (EKS,AKS,GKE 관리형 쿠버네티스)
----
+ 구성요소들이 kube-system 밑에 구성되어있다.
![image](https://user-images.githubusercontent.com/76584547/130314415-ce848416-f32a-478d-acb7-874ad704f6c3.png)

#### 쿠너베티스의 기본 철학
---
```
  MSA를 위한 각자의 일을 열심히 하는 철학
```

#### 파드가 배포되면?
---
![image](https://user-images.githubusercontent.com/76584547/130314547-c7b2b588-18eb-43e0-bba7-c3ddfd9bf6bf.png)


### 실제 쿠버네티스의 파드 배포 흐름
---

![image](https://user-images.githubusercontent.com/76584547/130314809-1cc2a811-e05e-4ddc-a2eb-15c91d3f18e9.png)
+ API 서버는 모든 것의 집합체이고 모든 것의 게이트웨이며, 모든 것의 시작과 끝이다.
+ excd는 api서버와 거의 1:1 동기화를 하기 때문에 에러가 날 경우 excd에서 힌트를 얻는다.
  + excd에 있는 정보를 복원을 하면서 데이터를 맞춰낸다.  
+ 5번 :kubelet이 워커노드에 파드 생성 요청을 하면 워커노드에 있는 컨테이너 런타임(도커)가 파드를 생성한다.
+ 8번 : 생성된 파드는 사용자와 통신을 하기위해 kube-proxy와 통신을 하게된다.



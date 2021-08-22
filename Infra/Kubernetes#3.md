## 쿠버네티스 인사이드 & 문제

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


### 파드에 문제가 생긴다면?
---

#### 파드를 실수로 지웠다면?
---
```
  ㅇ 파드만 배포된 경우
    - 지워진다
    - 단일 객체로 존재
    
  ㅇ 디플로이먼트 형태로 배포된 파드
    - 그 디플로이먼트가 파드를 유지하기 때문에 문제가 생기지 않는다.
    - 다중 파드를 가지고 있는 객체
    - 파드가 지워지면 다시 파드를 만든다.
```

#### 쿠버네티스가 파드를 대하는 자세
---
```
  - 파드가 죽을 수도 있지. 죽으면 다시 살아날 수 있는거지.
  - 해당 파드를 지우고 다시 생성한다. (옮겨지는 경우가 아님)
```

+ 파드를 삭제했을 경우 파드는 삭제되지만, 디플로이를 삭제할 경우 replicas가 3이기 때문에 파드 수를 유지하기 위해 1개를 다시 생성한다.
![image](https://user-images.githubusercontent.com/76584547/130316089-f946f48e-3617-4082-b7cb-03787e956b39.png)


+ 디플로이를 삭제하려면
```shell
  $ kubectl delete deployment del-deploy
```

#### kubelet 문제
---
```shell
  $ systemctl stop kubelet
  $ systemctl status kubelet
```

+ 워커노드에서 kubelet이 종료되어 있는 상태에서 배포하면 pending 상태가 유지된다.
+ 다시 kubelet을 실행하면 정상배포된다.

+ kubelet 종료 후 배포
![image](https://user-images.githubusercontent.com/76584547/130343112-5ef8d6cd-3f0f-4e64-83e3-23aa2d281095.png)

+ kubelet 재가동 후
![image](https://user-images.githubusercontent.com/76584547/130343124-dcabd19c-76ce-486e-adff-224ee66a2be6.png)


#### Container Runtime(도커) 문제
----
```shell
  $ systemctl stop docker (워커노드)
```

+ w1 - docker 종료 후 scaele로 배포
  + w1엔 배포를 시도조차하지 않는 것을 확인할 수 있다. 
![image](https://user-images.githubusercontent.com/76584547/130343216-41b92761-b65e-44af-8013-af8261b46e1d.png)

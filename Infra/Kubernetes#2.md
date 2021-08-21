### Pod 란?
---
```
  - 컨테이너들의 집합
  - 대부분은 단일 컨테이너
  - 하나의 컨테이너(하나의 도커)가 하나의 파드로 이루어지는 경우가 많다
```
![image](https://user-images.githubusercontent.com/76584547/130196633-da1dd5fa-8517-4e87-9728-4bed1a915fe9.png)


### Pod 배포
---
```shell
  $ kubectl run nginx --image=nginx
```
![image](https://user-images.githubusercontent.com/76584547/130197026-012e638a-8997-4c39-bfff-e0c2f7cc8bae.png)

+ Pod 확인
![image](https://user-images.githubusercontent.com/76584547/130197108-72703ebc-7cdd-40f6-aa60-1fb26c8c6bec.png)

+ 배포한 Pod의 IP 확인
```shell
  $ kubectl get pod -o wide
```
![image](https://user-images.githubusercontent.com/76584547/130197711-d48635de-fd5c-4db4-bfb7-80b27cd61eca.png)

+ 배포된 Nginx Pod 확인
```shell
  해당 ip에 curl을 호출하여 배포된 파드 확인

  $ curl 172.16.103.129
```
![image](https://user-images.githubusercontent.com/76584547/130197944-bda4bd19-1f4c-4d88-8409-4f77b9d9798c.png)

+ 디테일하게 확인
  + 할당된 ip와 이미지를 상세 확인할 수 있다 
```shell
  $ kubectl get pod nginx -o yaml
```
![image](https://user-images.githubusercontent.com/76584547/130198791-92299401-020c-49ef-8356-8c1f08f0e810.png)

![image](https://user-images.githubusercontent.com/76584547/130198820-10870622-782a-424c-b0a9-51b08ff4cffc.png)


#### 서비스(Service)
----
```
  파드를 외부에서도 접속하게 하는 서비스(Service)
```
+ 노드포트(Node Port) : 노드에 접속해서 Pod가 위치하는 곳을 찾는 것 (파드를 직접 찾는 것은 아님)
![image](https://user-images.githubusercontent.com/76584547/130311468-1c5e0171-b1df-4545-8794-25df8cc0ed01.png)

+ 파드 노출
```shell
  - 오픈하려고 하는 타입(type) : NodePort
  - 실제로 그 파드가 내부에 있는 컨테이너랑 통신하기 위한 포트 : 80
  
  $ kubectl expose pod nginx --type=NodePort --port=80
```

+ 노출된 파드 확인 
```shell
  - 밖으로 노출된 포트는 30451이라는 포트를 확인할 수 있다.
  
  $ kubectl get service
```

+ 노드 정보 확인
```shell
  - 노드를 알아야 파드에 접근할 수 있기 때문에 노드 정보가 필요하다.
  - 현재 배포할 때 192.168.10, 101, 102, 103으로 배포되게 설계가 되어있다. 
  - 마스터 - 10, w1 - 101, w2- 102, w3- 103 (외부에 노출되는 아이피)
  
  $ kubectl get nodes -o wide
```
![image](https://user-images.githubusercontent.com/76584547/130311677-e553447c-90a0-4dd3-8bc4-2f5e0e112fa0.png)


+ 브라우저에서 확인
```
  - 마스터 노드에 nginx의 파드를 확인할 수 있다.
  - 노드 아이피 (192.168.1.10) : 외부에 노출된 파드포트(30451)
```
![image](https://user-images.githubusercontent.com/76584547/130311783-ad658071-0c1b-4f5d-b9da-b31076f4434c.png)


#### 디플로이먼트(Deployment)
---
```
  파드를 여러개 모아놓은 단위
```

+ 파드, 디플로이 생성 방법
```
  ㅇ kubectl run 
    - 테스트 목적에 가깝다.
    - 1.1.8 이후 버전에서는 파드만 배포할 수 있고 디플로이는 배포할 수 없다.
    - 파드 하나만 배포할 때 유용함
    
  ㅇ kubectl create
    - 파드와 디플로이 모두 배포 가능
    
  ㅇ kubectl apply
    - 파드와 디플로이 모두 배포 가능
    - 하지만 파일이 필요하다.
```
![image](https://user-images.githubusercontent.com/76584547/130312010-f55150a8-6651-4265-a4d4-57cdd975d4d7.png)


+ nginx 디플로이 배포
```shell
  - 디플로이 배포시 : kubectl create deploment
  - 파드 배포 시 : kubectl create pod
  - 이름 명시 : deploy-nginx
  - 이미지 사용 : --image=nginx

  $ kubectl create deploment deploy-nginx --image=nginx
```

+ 파드 확인
```shell
  $ kubectl get pods
```
![image](https://user-images.githubusercontent.com/76584547/130312149-758ad311-7d22-4343-837d-2d672318be3d.png)

+ 배포 파드 여러개로 늘리기
```
  ReplicaSet의 replicas 가 디폴트로 파드 1개로 되어있는데 이를 늘려야한다.
```

+ 2개가 더 늘어나서 3개가 된 것을 확인할 수 있다.
```shell
  $ kubectl scale deployment deploy=nginx --replicas=3
```

![image](https://user-images.githubusercontent.com/76584547/130312605-1de0af92-5436-4691-88bf-36ea990cf6f7.png)

### 로드밸런서
----
```
  현재 NodePort로 배포하고 있지만 이는 가장 좋은 방법이 아니다. 
  로드밸런서를 이용해서 구성해야할 것
```

+ 로드밸런서의 좋은 점
```
  1. 노드포트는 노드의 IP를 다 알아야하는데, 로드밸런서는 고유의 ip를 만들어서 사용자에게 알려줄 수 있다.
  그래서 노드의 ip를 오픈하는 부담이 없다.
  2. 로드밸런서가 가야할 경로를 최적화할 수 있다.
  3. MeetalLB라는 로드밸런서 타입을 선언할 수 있다.
```
![image](https://user-images.githubusercontent.com/76584547/130313156-ca139596-777b-4874-ae1a-ad941c5d3d1c.png)

+ 현재 프로젝트에서 metalLb 설치 (이미 설치해놓음)
```shell
  $ kubectl apply -f ~/_Lecture_k8s_starter.kit/ch2/2.4/metallb.yaml
```
![image](https://user-images.githubusercontent.com/76584547/130313220-3b85770d-d9e8-4cf7-8524-12867bdd47a3.png)


+ chk-hn의 디플로이먼트 생성
```shell
  $ kubectl create deployment chk-hn --image=sysnet4admin/chk-hn
```

+ 여러개 생성
```shell
  $ kubectl scale deployment chk-hn --replicas=3
```
![image](https://user-images.githubusercontent.com/76584547/130313292-b41b7da9-a593-47ca-81dd-50fab2c4e42c.png)


+ 생성한 디플로이먼트를 노출하기
```shell
  - 디플로이먼트 노출 : kubectl expose deployment
  - 이름 : chk-hn
  - 타입 (로드밸런서) : --type=LoadBalancer
  - 포트 : --port=80

  $ kubectl expose deployment chk-hn --type=LoadBalancer --port=80
```

+ 192.168.1.11 가 EXTERNAL-IP로 설정이 되었기 때문에 더이상 노드포트를 알려줄 필요가 없다.
  + 고정된 해당 IP로 접근 가능하다. 
![image](https://user-images.githubusercontent.com/76584547/130313383-5eefb565-3a90-4131-9ab7-c0d0f3f6e7ac.png)

#### 배포된 것 삭제
---
```shell
  $ kubectl delete service 서비스명
  $ kubectl delete pod 서비스명
  $ kubectl delete deployment 서비스명
  $ kubectl delete -f ~/_Lecture_k8s_starter.kit//ch2/2.4/metallb.yaml
```
![image](https://user-images.githubusercontent.com/76584547/130313827-48f8f2f8-c1fd-4c28-a9b5-530e4af3a8fd.png)



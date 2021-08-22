### 쿠버네티스 오브젝트란?
----
```
  - 상태를 가지고 있는 것들(실제론 코드로 되어있음)
  - 추구하는(spec) 상태를 가지고 있는 것
```
![image](https://user-images.githubusercontent.com/76584547/130343956-f64f3673-c6e0-4915-bce6-4ad7e0c6fed7.png)

+ 추구하는 상태와 현재 상태를 맞추려는 것
![image](https://user-images.githubusercontent.com/76584547/130343989-47ded960-1cb1-4442-abdf-b5a632f37a98.png)

+ replicas를 추구하는 상태 & 현재 상태
![image](https://user-images.githubusercontent.com/76584547/130344027-1ffc324c-f52b-46e3-81ea-82846bcb6945.png)

##### del-deploy 추구하는 상태 변경
----
```shell
  $ kubectl edit deployment del-deploy

  $
```
![image](https://user-images.githubusercontent.com/76584547/130344080-2d75d5ce-8cd7-4f9f-a9fa-94a32cba93ff.png)

+ spec의 replicas 가 현재 9로 되어있지만 이를 3으로 변경
![image](https://user-images.githubusercontent.com/76584547/130344094-9f75fc82-d9ff-4485-9853-e2d893027ba4.png)

![image](https://user-images.githubusercontent.com/76584547/130344102-ee3a8048-ef5f-471a-b4b2-5e652771a2ea.png)


+ 9개의 파드에서 3개의 파드로 줄이는 것을 확인할 수 있다.
![image](https://user-images.githubusercontent.com/76584547/130344116-3c845bc1-7171-4d82-9a35-44d20ca80273.png)
![image](https://user-images.githubusercontent.com/76584547/130344120-ef19213a-409e-4d5c-835d-fe4103f534a9.png)


### 볼륨 오브젝트
---
```
  - 영속적인 데이터를 보존하기 위한 것
  - 파드가 이곳 저곳으로 돌아다니면 안되기 때문에 파드를 만들 때 볼륨을 붙이는 구조 필요
```

#### NFS 서버
----
```
  네트워크 파일 시스템 : 모두 다 같이 사용할 수 있는 볼륨 시스템을 만들 수 있다.
  워커노드 3개가 모두 같은 것을 바라봐야하기 때문에 사용
```
+ 미리 만들어둔 쉘을 이용해서 nfs 생성
  + nfs shared에 워커노드 3개를 공유할 수 있는 곳 : 192.168.1.0/24
![image](https://user-images.githubusercontent.com/76584547/130344524-f914aa63-b067-4781-9bd0-02ba5f677b00.png)

+ 볼륨은 영속적으로 데이터가 남아있다
+ ![image](https://user-images.githubusercontent.com/76584547/130345354-8b6c3fcf-4bb8-43d3-8d55-a4dd7bb7d643.png)




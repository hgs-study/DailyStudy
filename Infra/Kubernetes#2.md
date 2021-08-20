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

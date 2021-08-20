## Kubernetes
+ Docker vs Kubernetes
``` 
  - 컨테이너 하나 띄워서 사용해야지 => 도커
  - 0월 0시에 100개의 컨테이너를 자동으로 생성해야지 => 쿠버네티스
  - 도커는 '기술적인 개념이자 도구'
  - 쿠버네티스는 '도커를 관리하는 툴'
```

+ Orchestration 이란?
=> 쿠버네티스는 '컨테이너 오케스트레이션 툴' 이다.
``` 
  - 컨테이너 역시 그 수가 많아지게 되면 관리와 운영에 있어서 어려움이 따른다.
  - 오케스트레이션 엔진을 통해 컨테이너의 생성과 소멸, 시작 및 중단 시점 제어, 스케줄링, 로드 밸런싱, 클러스터링 등 컨테이너로 어플리케이션을 구성하는 모든 과정을 관리할 수 있다
```

+ Kubernetes 특징 
``` 
  - 자동화된 복구(self-healing)
  - 로드 밸런싱(Road balancing)
  - 무중단(Fault tolerance-FT) 서비스
  - 호환성(Vendor Lock In 해결)
```

### Kubernetes란?
---
```
  컨테이너( 대표적으로 도커)를 관리하는 툴
```

### Vagrant 란?
---
```
  Vitual Box에 쿠버네티스 환경을 구성하기 위해 스크립트 코드를 보내면 Virtual Box에 여러 Node를 구성해준다 
```
+ 출처 : https://www.vagrantup.com/
![image](https://user-images.githubusercontent.com/76584547/130187009-539b0b51-9fd5-4b55-9e8e-1c80cfee1195.png)


### VirtualBox란?
---
```
  리눅스 환경을 구성하는 툴 (vm ware 등등 있음)
```
+ 출처: https://www.virtualbox.org/
![image](https://user-images.githubusercontent.com/76584547/130187435-44a5d8db-ace3-4986-a4b7-b7dd06811039.png)




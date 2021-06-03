## SpringCloud

### SpringCloud란?
-----
```
   - 마이크로서비스의 개발, 배포, 운영에 필요한 아키텍처를 쉽게 구성할 수 있도록 지원하는 Spring Boot기반의 프레임워크
   - 자신이 사용하고 있는 스프링부트 버전과 스프링 클라우드 버전이 연관이 있다
```

![image](https://user-images.githubusercontent.com/76584547/120656125-51caaf00-c4be-11eb-8442-ccd8416b336d.png)

+ SpringCloud는 서비스를 제공
![image](https://user-images.githubusercontent.com/76584547/120656310-7aeb3f80-c4be-11eb-833f-d892b6f9a6a8.png)

+ 환경 설정 관리
  + Spring Cloud Config Server
  + 변경된 값이 있더라도 외부 저장소(git)에 저장된 자료만 변경하면 서비스를 빌드/재배포 할 필요가 없다.
  + ex) 게이트웨이 ip, 서버가 가지고 있어야할 토큰 정보 등

![image](https://user-images.githubusercontent.com/76584547/120656524-a706c080-c4be-11eb-9c35-769bbd79a9d0.png)


+ 서비스 위치 정보 확인
  + Naming Server(Eureka)
  + ex) 넷플릭스 유레카 

![image](https://user-images.githubusercontent.com/76584547/120658340-4e382780-c4c0-11eb-81f0-f9126c065520.png)


+ 부하 부산 (로드 밸런싱)
  + Ribbon (Client Side)
  + Spring Cloud Gateway (넷플릭스 juul을 사용했지만 최신 스프링 클라우드 버전의 Spring Cloud Gateway 사용 권장

+ RestTemplate
  + Rest 통신을 위한 것
  + FeignClient 

+ 외부 모니터링 서비스
  + 시각화, 모니터링, 로그 추적
  + Zipkin Distibuted Tracing
  + Netflix API Gateway
  

+ 장애 복구
  +  Netflix Hystrix

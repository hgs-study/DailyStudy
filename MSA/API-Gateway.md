## API-Gateway

### API-Gateway란?
```
  - 사용자가 설정한 라우팅 설정에 따라서 각각 엔드포인트로 클라이언트 대신 요청해서 대신 응답을 받고 요청을 해주는 Proxy 역할
  - 시스템 내부 구조는 숨기고 외부 요청에 대해 적절한 형태로 가공해서 응답할 수 있다
```
![image](https://user-images.githubusercontent.com/76584547/120923404-4cad7000-c709-11eb-8284-aa5b959ec6d0.png)


### Netflix Ribbon
  + Spring Cloud에서의 MSA간 통신
    + RestTemplate 
      + 전통적인 RestAPI 호출 방식
      ![image](https://user-images.githubusercontent.com/76584547/120923539-102e4400-c70a-11eb-946e-4157e813cfef.png)
    
    + Feign Client
    ![image](https://user-images.githubusercontent.com/76584547/120923620-8468e780-c70a-11eb-8be0-5880a4d0b086.png)

    
   + Ribbon : Client side Load Balancer (Netflix) - 서버 x, 클라이언트 사이드에 존재
      + 스프링 초창기에 사용
      + 비동기 처리(Functional api, 리액트자바 등)가 안 되기 때문에 최근에 잘 사용하지 않음
      + 서비스 이름으로 호출
      + Health Check
      + Spring Cloud Ribbon은 Spring Boot 2.4에서 Maintenance 상태 (더이상 지원하지 않는 보류 상태)

   + Zuul : 
    + API Gateway 역할
    + Routing 역할 
    + Spring Cloud Ribbon은 Spring Boot 2.4에서 Maintenance 상태 (더이상 지원하지 않는 보류 상태)

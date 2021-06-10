### SpringCloud-Gateway
----
```
  - 기존 Netflix Zuul은 비동기 처리하지 못했지만 2점대 버전으로 올라와서 가능하다고 했으나 라이브러리 호환성 문제가 남아있다.
  - SpringCloud-Gateway 비동기 처리 가능하며 SpringBoot 2.4이상에서 사용할 수 있다.
  - 동기서버 (Tomcat) vs 비동기 서버(Netty)
```
+ 의존성


  ![image](https://user-images.githubusercontent.com/76584547/120929435-f699f600-c723-11eb-86b3-4ff7317daff0.png)

+ 설정파일 


  ![image](https://user-images.githubusercontent.com/76584547/120929472-1d582c80-c724-11eb-8c0c-235bac7df53c.png)

  + cloudgate way predicates는 해당 service uri 뒤에 붙어서 매핑된다.

<br/>


### Filter
----
+ API Gateway에서 필터 사용


![image](https://user-images.githubusercontent.com/76584547/121021204-1987e100-c7dc-11eb-81a3-d6ce4bafea11.png)

![image](https://user-images.githubusercontent.com/76584547/121030478-79828580-c7e4-11eb-9d98-a8ad6e8377a2.png)
<br/>

+ application.yml 설정 주석 (config파일로 사용하려고)


![image](https://user-images.githubusercontent.com/76584547/121026733-5a362900-c7e1-11eb-8ce9-7d2285963eae.png)


+ FilterConfig 생성

![image](https://user-images.githubusercontent.com/76584547/121026946-88b40400-c7e1-11eb-884c-e55f7432d04b.png)

  + RouteLocator를 반환해야 application.yml에서 설정한 것처럼 라우터 역할을 한다.
  + RouteLocatorBuilder 인자로 받아 빌더를 라우트 설정 후 리턴
  + 각각의 route path와 uri로 라우트 시키고 filter를 작성한다 (request, response header에 테스트 문구 삽입)
<br/>


+ 결과
![image](https://user-images.githubusercontent.com/76584547/121027328-d7619e00-c7e1-11eb-91cb-a7c7a745cef5.png)


+ application.yml 활용


![image](https://user-images.githubusercontent.com/76584547/121030113-26a8ce00-c7e4-11eb-8fee-4eee7bc2b320.png)
<br/>

### Custom Filter
----
```
  - 사용자 지정 필터 만들 수 있다.
  - Pre, Post 필터 지정 가능
```
+ CustomFilter


![image](https://user-images.githubusercontent.com/76584547/121520721-34ea2a80-ca2e-11eb-8b02-d1318ec7cfbd.png)


+ 상단 박스가 Pre필터, 하단 박스가 Post 필터 구성
+ AbstractGatewayFilterFactory를 상속 받아서 필터 구현

<br/>

+ application.yml


![image](https://user-images.githubusercontent.com/76584547/121520868-64993280-ca2e-11eb-8fc7-a3879e92f253.png)


+ 결과

![image](https://user-images.githubusercontent.com/76584547/121520927-74b11200-ca2e-11eb-92fd-bb75379ac824.png)
<br/>

+ request id가 1씩 증가하는 걸 알 수 있으며, response 상태 코드도 알 수 있다.


### Global Filter
----
```
  - 서비스 전역에 해당되는 필터를 만들 수 있다.
  - 실행 순서는 GlobalFilter가 먼저 실행 -> CustomFilter pre-> CustomFilter post -> GlobalFilter 실행
```
+ GlobalFilter

![image](https://user-images.githubusercontent.com/76584547/121523260-fa35c180-ca30-11eb-8d2b-21d6b43b9718.png)
<br/>

+ 이너클래스 Config에 baseMessage, preLogger, postLogger 추가


+ application.yml


![image](https://user-images.githubusercontent.com/76584547/121523614-5d275880-ca31-11eb-9d28-ed8df04f7637.png)
<br/>

+ default-filters를 지정함으로서 글로벌 필터로 사용 가능
+ args를 지정할 수 있다.
+ 나중엔 yml로 관리 안 하고 외부 파일에서 관리해서 마이크로 서비스가 재배포 되지 않아도 필터는 변경할 수 있도록 외부에서 관리

<br/>

+ 결과
```
  실행 순서는 GlobalFilter가 먼저 실행 -> CustomFilter pre-> CustomFilter post -> GlobalFilter 실행
```
![image](https://user-images.githubusercontent.com/76584547/121523820-99f34f80-ca31-11eb-8014-fc337a88c100.png)


### Logging Filter
```
  - 로깅 필터(커스텀 필터)를 우선순위를 정해서 출력할 수 있다. (글로벌필터보다 빠른 순위로 필터링 가능)
```

+ LoggingFilter
```
  - 이번엔 GatewayFilter의 자식인 OrderedGatewayFilter를 생성하여 우선순위(Order) 설정 가능
  - Ordered.HIGHEST_PRECEDENCE : HIGHEST라서 가장 먼저 실행 가능  <-> LOWEST_PRECEDENCE
```
<br/>

![image](https://user-images.githubusercontent.com/76584547/121527774-bbeed100-ca35-11eb-901c-fbd40fa96e3d.png)
<br/>

+ application.yml
```
  1. 서비스 filters에 name을 붙이고 필터명 기입
  2. arg(baseMessage,preLogger,postLogger) 설정 가능
```
<br/>

![image](https://user-images.githubusercontent.com/76584547/121528325-50593380-ca36-11eb-9639-9513a3fd5a5d.png)
<br/>

+ 결과
```
  Ordered.HIGHEST_PRECEDENCE : HIGHEST라서 GlobalFilter보다 우선 시작
```
<br/>

![image](https://user-images.githubusercontent.com/76584547/121528144-2bfd5700-ca36-11eb-8309-a7f2f5cc592c.png)



### Load Balancer
```
  클라이언트에서 요청이 오면 Discovery에서 요청 api에 맞는 서비스의 주소를 반환해주고 요청 응답이 이루어진다.
```

![image](https://user-images.githubusercontent.com/76584547/121539034-42101500-ca40-11eb-9fdc-cf668aaf8721.png)

<br/>


+ Api-Gateway
```
  - eureka 관련 설정(등록,조회)와 defaultZone을 설정
  - routes에 uri이 다른 Service의 spring.application.name 과 매핑된다.
  - lb://(서비스 어플리케이션 이름)  ex) lb://MY-FIRST-SERVICE
```
<br/>
![image](https://user-images.githubusercontent.com/76584547/121539221-6f5cc300-ca40-11eb-9052-8b7931807c08.png)
<br/>

![image](https://user-images.githubusercontent.com/76584547/121539613-cb274c00-ca40-11eb-8be5-ec90ab6bde6e.png)
<br/>

+ First-Service
```
  서비스는 spring.application.name을 설정한다.
```

![image](https://user-images.githubusercontent.com/76584547/121539291-826f9300-ca40-11eb-9dd4-efbaa89657bd.png)

<br/>

+ Second-Service
```
  서비스는 spring.application.name을 설정한다.
```
<br/>

![image](https://user-images.githubusercontent.com/76584547/121539380-97e4bd00-ca40-11eb-939e-cbed34aa9658.png)

<br/>


+ 할당하는 포트 확인
```
  - 똑같은 서비스가 여러개(2개 이상) 실행되었을 때 어떻게 요청을 서버에 할당하는지
  - (A 서비스가 3개 띄어져있을 경우 어떤 서버로 요청/응답하는지?)
  - 라운드 로빈 방식으로 한 번씩 돌아가면서 요청한다.
```

+ 실습
<br/>
```
  랜덤 포트 사용
```

<br/>

![image](https://user-images.githubusercontent.com/76584547/121548628-227cea80-ca48-11eb-95f1-9eb0fffa36aa.png)

<br/>

```
  어떤 포트로 요청들어오는지 로그 확인
```

<br/>

![image](https://user-images.githubusercontent.com/76584547/121548735-3de7f580-ca48-11eb-9a6a-77962fb7960f.png)

<br/>

+ 결과
```
  총 12번 실행했는데 
  11341 -> 10017 -> 11341 -> 10017 -> 11341 -> 10017 -> ...
  이런식으로 한 번씩 서버를 할당한다 (라운드 로빈 방식)
```
<br/>

![image](https://user-images.githubusercontent.com/76584547/121548816-4d673e80-ca48-11eb-801f-506297ba30f3.png)

![image](https://user-images.githubusercontent.com/76584547/121548862-548e4c80-ca48-11eb-8fb2-51f1c3a219be.png)

<br/>

+ 라운드 로빈이란?
```
- 라운드 로빈 스케줄링(Round Robin Scheduling, RR)은 시분할 시스템을 위해 설계된 선점형 스케줄링의 하나로서, 
   프로세스들 사이에 우선순위를 두지 않고, 순서대로 시간단위(Time Quantum/Slice)로 CPU를 할당하는 방식의 CPU 스케줄링 알고리즘입니다.

- 순서대로 시간단위(Time Quantum/Slice)로 CPU를 할당 
```


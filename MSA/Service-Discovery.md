## Service Discovery

### Service Discovery란?
-----
```
  - 서비스를 등록 조회할 수 있어서 요청 받은 서비스가 어디 있는 지 확인할 수 있다
  - 외부에서 다른 서비스들이 마이크로 서비스를 사용하기 위한 전화번호부 같은 역할
  - 서비스 등록, 검색
  - key, value
    - A서버, ASSAF0
    - B서버, SADFAS
  - 요청 서비스를 찾아주는 역할
```

![image](https://user-images.githubusercontent.com/76584547/120660538-627d2400-c4c2-11eb-9800-717e1c485f18.png)


### Netflix Eureka
-----
```
  - Netflix 클라우드 기술들을 자바 스프링 재단에 기부해서 사용할 수 있다
```

+ Eureka 서버 등록
![image](https://user-images.githubusercontent.com/76584547/120662335-ff8c8c80-c4c3-11eb-9c9c-853cf45ce508.png)

+ Eureka 서버 설정
![image](https://user-images.githubusercontent.com/76584547/120663605-213a4380-c4c5-11eb-845e-aa6eb92d2bcc.png)



+ http://localhost:8761 (유레카 서버 기본 화면)

![image](https://user-images.githubusercontent.com/76584547/120663536-0ff13700-c4c5-11eb-9161-e3b9261d8183.png)


### [수동 포트 변경] Discovery Client 연동
-----
  + [#1] 하위에 Discovery Client(user-service)를 붙임
  
  ![image](https://user-images.githubusercontent.com/76584547/120832056-2cf83980-c59b-11eb-9008-df0b7a0cb06a.png)


  + [#2] 포트 다른 여러 서버 띄우기 (인텔리제이)
  
  ![image](https://user-images.githubusercontent.com/76584547/120833664-0509d580-c59d-11eb-8fc5-c63edfcbede3.png)

  + [#2] APP 2개 띄었을 때

  ![image](https://user-images.githubusercontent.com/76584547/120834346-d93b1f80-c59d-11eb-951d-aa02d81fb489.png)

  + [#3] 명령어로 서버 추가 실행 (인텔리제이 터미널)
    + mvn spring-boot:run -Dspring-boot.run.jvmArguments='-Dserver.port=9003'


  ![image](https://user-images.githubusercontent.com/76584547/120835942-f4a72a00-c59f-11eb-902c-92422ec7af9b.png)
  ![image](https://user-images.githubusercontent.com/76584547/120836163-3afc8900-c5a0-11eb-9606-a2afc1c2a31f.png)
  
  + [#4] cmd
    + mvn clean (target 삭제)
    + mvn compile package (컴파일 후 패키지 생성)
    + 빌드 성공 후 jar 파일이 만들어짐
    ![image](https://user-images.githubusercontent.com/76584547/120836944-1ead1c00-c5a1-11eb-8cce-a89ccbd23ab1.png)
    + jar 실행 : java -jar -Dserver.port=9004 ./target/user-service-0.0.1-SNAPSHOT.jar

    ![image](https://user-images.githubusercontent.com/76584547/120837491-b7439c00-c5a1-11eb-9b6e-b531100cb378.png)

    


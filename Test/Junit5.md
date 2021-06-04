## Junit5

### Junit5란?
-----
```
  자바 개발자가 가장 많이 사용하는 테스팅 프레임 워크
    - 단위 테스트를 작성하는 93% 자바 개발자가 사용 중 (젯브레인 리포트 中)
    - 자바 8이상을 필요로 함
    - 대체제 : TestNG, Spock, 등등
```
![image](https://user-images.githubusercontent.com/76584547/120640178-212e4980-c4ad-11eb-8152-143e2f88dfaf.png)


  + 세부 모듈을 가지고 있다.
    + Platform : 테스트를 실행해주는 런처 제공. TestEngine API 제공 
    + Jupiter : TestEngine API의 구현체로 Junit 5를 제공
    + Vintage : Junit 4와 3을 지원하는 TestEngine 구현제

![image](https://user-images.githubusercontent.com/76584547/120641428-8afb2300-c4ae-11eb-8886-9464b7358c15.png)
![image](https://user-images.githubusercontent.com/76584547/120642053-605d9a00-c4af-11eb-8d23-39669b205343.png)

  + 스프링부트 2.2 이상을 사용할 경우
    + 'org.springframework.boot:spring-boot-starter-test'을 의존성으로 가져올 경우 junit5버전을 디폴트로 가져옴 
    + 별다른 의존성을 추가하지 않아도 사용 가능


![image](https://user-images.githubusercontent.com/76584547/120641838-1674b400-c4af-11eb-9371-1b7ed3b49f80.png)
  + 앞에 public을 붙이지 않아도 된다.
    + junit 4는 public안에 public 메서드만 실행할 수 있었다
    + 리플렉션을 사용하기 때문에


![image](https://user-images.githubusercontent.com/76584547/120642927-77e95280-c4b0-11eb-8171-1df27d6a03cf.png)
![image](https://user-images.githubusercontent.com/76584547/120643026-98b1a800-c4b0-11eb-96c2-79301775758d.png)
  
  + @BeforeAll
    + 테스트 클래스 안에 있는 여러 테스트가 모두 실행하기 전에 딱 한 번 호출이 된다.
    + 반드시 static 메서드를 사용해야한다. private(x) defalut(o) 리턴타입(x)
   
  + @AfterAll
    + 모든 테스트 실행 이후 한번만 호출된다.
    + 반드시 static 메서드를 사용해야한다.

  + @BeforeEach
    + 모든 테스트를 실행하기 이전에 한 번 실행된다.
    + 스태틱일 필요는 없다.

  + @AfterEach
    + 모든 테스트를 실행하기 이후에 한 번 실행된다.
    + 스태틱일 필요는 없다.


### Assert
---

![image](https://user-images.githubusercontent.com/76584547/120801504-3625de00-c57c-11eb-80e2-98b5f220ab68.png)

+ 람다 서플라이어
![image](https://user-images.githubusercontent.com/76584547/120801620-5a81ba80-c57c-11eb-9e09-dcf7bcdfd0b9.png)

+ assertAll : 여러 테스트 한 번에 가능
![image](https://user-images.githubusercontent.com/76584547/120803529-9c136500-c57e-11eb-9d33-d98d4ed78261.png)

![image](https://user-images.githubusercontent.com/76584547/120803568-a6356380-c57e-11eb-97d0-8e0bb093a496.png)

 




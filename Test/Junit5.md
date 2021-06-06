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


### Assertion
---

![image](https://user-images.githubusercontent.com/76584547/120801504-3625de00-c57c-11eb-80e2-98b5f220ab68.png)

+ 람다 서플라이어
![image](https://user-images.githubusercontent.com/76584547/120801620-5a81ba80-c57c-11eb-9e09-dcf7bcdfd0b9.png)

+ assertAll : 여러 테스트 한 번에 가능
![image](https://user-images.githubusercontent.com/76584547/120803529-9c136500-c57e-11eb-9d33-d98d4ed78261.png)

![image](https://user-images.githubusercontent.com/76584547/120803568-a6356380-c57e-11eb-97d0-8e0bb093a496.png)

 
+ assertThrows
![image](https://user-images.githubusercontent.com/76584547/120804326-789cea00-c57f-11eb-9605-159e58514deb.png)

+ assertTimeout
  + 코드블록({ }) 안에 로직이 300밀리 세컨드로 설정되어있는데 이 300밀리 세컨드를 다 기다리고 테스트가 완료된다.
![image](https://user-images.githubusercontent.com/76584547/120805812-28bf2280-c581-11eb-836a-02114b4c472e.png)

+ assertTimeoutPreemptively
  + 코드블록({ }) 안에 로직이 300밀리 세컨드로 설정되어있는데  이 테스트는 100밀리 세컨드가 넘으면 즉시 완료된다
  + 문제점 : ThreadLocal을 사용하는 로직이 있으면 예상치 못한 에러를 발생시킬 수 있다.
  + ThreadLocal : Spring Transaction은 ThreadLocal을 사용하는데, 다른 쓰레드에선 공유가 안 된다.
  +   -> 스프링 트랜잭션 설정이 제대로 테스트 안 될 수가 있다. (롤백이 안 되고 디비에 반영될 수 있다.)
![image](https://user-images.githubusercontent.com/76584547/120805939-43919700-c581-11eb-9424-fb2c118992e0.png)


+ assertThat
  + assertThat(assertj 라이브러리 사용, 이건 junit은 아님, 취향 것 사용)
![image](https://user-images.githubusercontent.com/76584547/120806386-c0247580-c581-11eb-8afb-2fc6ad5f441f.png)


+ Tag
  + 테스트마다 Tag 설정 가능 & 인텔리제이에서 특정 tag만 실행 가능
  ![image](https://user-images.githubusercontent.com/76584547/120915887-486d5c80-c6e1-11eb-9280-e2f97531791b.png)
  ![image](https://user-images.githubusercontent.com/76584547/120915897-59b66900-c6e1-11eb-8007-76e477c79e70.png)
  
  
+ 커스텀 태그
  + 커스텀 어노테이션을 사용할 경우 <-> 기존의 @TAG("fast")는 타입 세이프하지 않다. fasd, fadt 등 오타가 날 수 있다.
  ![image](https://user-images.githubusercontent.com/76584547/120916117-a189c000-c6e2-11eb-8e4f-c445147d7b3c.png)
  ![image](https://user-images.githubusercontent.com/76584547/120916125-ac445500-c6e2-11eb-91c4-4980ef72a319.png)


+ RepeatedTest (테스트 반복)
  + 반복 테스트
  ![image](https://user-images.githubusercontent.com/76584547/120918010-2aa5f480-c6ed-11eb-9e41-4ac3611b17d3.png)
  ![image](https://user-images.githubusercontent.com/76584547/120918015-32fe2f80-c6ed-11eb-868e-f928d62a506b.png)


+ ParameterizedTest, ValueSource (테스트 반복)
  +  파라미터 테스트 (파라미터 갯수만큼 테스트)
  ![image](https://user-images.githubusercontent.com/76584547/120918219-4067e980-c6ee-11eb-981c-8014ab4e7c11.png)
  ![image](https://user-images.githubusercontent.com/76584547/120918221-44940700-c6ee-11eb-9bf7-9b345899fd8b.png)

+ EmptySource & NullSource
  + 비어있는 문자열을 테스트에 추가 / Null 추가 이것을 합친 것이 => @NullAndEmptySource
  ![image](https://user-images.githubusercontent.com/76584547/120918637-5080c880-c6f0-11eb-9d7a-757256208770.png)
  ![image](https://user-images.githubusercontent.com/76584547/120918644-5d052100-c6f0-11eb-8166-d775178d9e18.png)


+ Converter (테스트 반복)
  + 커스텀한 타입(Study)으로 변환하고 해서 파라미터를 받고 싶을 땐 SimpleArgumentConverter 상속받은 구현체를 사용한다. (인자 하나) 
  ![image](https://user-images.githubusercontent.com/76584547/120919544-2aa9f280-c6f5-11eb-8fe7-b6d192c09ad9.png)
  

+ 복수 인자 (테스트 반복)
  + 커스텀한 타입(Study)을 여러개 받고 싶을 때
  + [#1] : 타입 변수 인자로 받아옴
  ![image](https://user-images.githubusercontent.com/76584547/120919630-8f654d00-c6f5-11eb-9d24-634ea7590d19.png)
  ![image](https://user-images.githubusercontent.com/76584547/120919631-92f8d400-c6f5-11eb-9ea3-4c3567aa40d8.png)

  + [#2] : ArgumentsAccessor argumentsAccessor 활용
  ![image](https://user-images.githubusercontent.com/76584547/120919734-1adede00-c6f6-11eb-9cd2-b107700a38be.png)
  ![image](https://user-images.githubusercontent.com/76584547/120919738-1fa39200-c6f6-11eb-8177-5bddab9d400b.png)
  
  + [#3] : ArgumentsAggregator 활용 더 줄일 수 있다 (※퍼블릭 클래스이거나 이너 스태틱 클레스여야한다.)
  ![image](https://user-images.githubusercontent.com/76584547/120919850-b4a68b00-c6f6-11eb-86d8-8e41777579b5.png)





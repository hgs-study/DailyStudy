### 테스트 인스턴스
----
  + 테스트 메서드마다 클래스 인스턴스를 새로 생성한다 (※이유 : 테스트간의 의존성을 없애기 위해)


  ![image](https://user-images.githubusercontent.com/76584547/120920411-955d2d00-c6f9-11eb-97b7-268fb6228e52.png)
  ![image](https://user-images.githubusercontent.com/76584547/120920435-b756af80-c6f9-11eb-96d4-5e5d184a5fbc.png)
  ![image](https://user-images.githubusercontent.com/76584547/120920438-bcb3fa00-c6f9-11eb-9882-31fcb2df098c.png)
    + value 값이 늘어나지 않고 0이 출력되는 것을 확인할 수 있다.


  + @TestInstance
    + 클래스마다 의존성 생성하기 때문에 이젠 여러 메서드가 하나의 인스턴스를 공유한다
    + @BeforeAll, @AfterAll이 원래는 static이여야하는데 static을 안 써도 된다.


    ![image](https://user-images.githubusercontent.com/76584547/120920656-d73aa300-c6fa-11eb-9c85-f35687838d23.png)
    ![image](https://user-images.githubusercontent.com/76584547/120920660-dc97ed80-c6fa-11eb-9c07-3d51a9176a32.png)
    + 인스턴스 하나를 공유하기 때문에 value가 1인 것을 확인할 수 있다.


### 테스트 순서
----
  + junit5부터는 메소드 순서대로 테스트가 실행되긴 하지만 100%는 아니다 내부 로직에 따라 다를 수 있다.
  + @TestMethodOrder(MethodOrderer.OrderAnnotation.class) , @Order(순서) 사용


  ![image](https://user-images.githubusercontent.com/76584547/121009215-db37f500-c7ce-11eb-9d9a-1066ff58311a.png)

  + 결과


  ![image](https://user-images.githubusercontent.com/76584547/121009283-eee35b80-c7ce-11eb-8435-eab4f6e68a23.png)

  + 한 인스턴스로 상태를 공유할 때 사용할 듯하다 


### 테스트 설정파일
----
+ 테스트 설정파일
  + [test]-[java]-[resources]-[junit-platform.properties] 생성 (java 패키지와 같은 레벨)

  ![image](https://user-images.githubusercontent.com/76584547/121014279-c5c5c980-c7d4-11eb-8622-caa2a8dde60a.png)


  + [Product Structure] - [ resources]선택 - [Test Resources]

  ![image](https://user-images.githubusercontent.com/76584547/121014347-d70ed600-c7d4-11eb-91c8-c5ff7213d7a0.png)
  
+ 테스트 인스턴스 라이프사이클 설정
  + junit.jupiter.testinstance.lifecycle.default = per_class
  
  ![image](https://user-images.githubusercontent.com/76584547/121015329-d62a7400-c7d5-11eb-98ba-c42706934875.png)


  ![image](https://user-images.githubusercontent.com/76584547/121015343-dc205500-c7d5-11eb-8afb-6127a2e4c973.png)

  
+ @Disabled 무시하고 실행하기
  + junit.jupiter.conditions.deactivate = org.junit.*DisabledCondition


  ![image](https://user-images.githubusercontent.com/76584547/121015382-e6daea00-c7d5-11eb-81a0-0239e3eea247.png)
  
  ![image](https://user-images.githubusercontent.com/76584547/121015402-ee9a8e80-c7d5-11eb-990e-a810495a7664.png)


+ 디스플레이 네임 (언더스코어 공백 치환)
  + junit.jupiter.displayname.generator.default = \
  org.junit.jupiter.api.DisplayNameGenerator$ReplaceUnderscores
  + @DisplayNameGeneration 보다는 메서드에 있는 @DisplayName이 우선순위가 더 높다
  ![image](https://user-images.githubusercontent.com/76584547/121015956-88623b80-c7d6-11eb-82cc-e74c986cd0a5.png)


### Junit5 확장 모델
----
```
  - Junit 4의 확장 모델은 @RunWith(Runner), TestRule, MethodRule
  - Junit 5의 확장 모델은 단 하나, Extension
  1. 선언적 방법 => @ExtendWith(FindSlowTestExtension.class)
  2. 프로그래밍적 방법 => @RegisterExtension
```

+ 확장팩 등록 방법
  + 선언적인 등록 @ExtendWith 
  ```
    1초 이상 걸리는 메서드 확인하는 Extension Method 
  ```
  <BR/>
  
  ![image](https://user-images.githubusercontent.com/76584547/121802087-c5ff1280-cc75-11eb-9383-7423005f6d20.png)

  <BR/>
  + 선언적인 등록 방법 : @ExtendWith(FindSlowTestExtenstion.class) 
  ![image](https://user-images.githubusercontent.com/76584547/121802082-bed80480-cc75-11eb-9928-6269610389fc.png)


  + 결과 
  <br/>
  
  ![image](https://user-images.githubusercontent.com/76584547/121802131-0494cd00-cc76-11eb-8428-0be511969bbe.png)
  
  + 1초 이상 걸리는 메서드는 @SlowTest 권장
  ```java
    - 리플렉션을 이용한 @SlowTest 어노테이션 확인 후 null 값이면 권장 메세지 표시

    Method requiredTestMethod = context.getRequiredTestMethod();   // 리플렉션을 활용한 방법
    SlowTest annotation = requiredTestMethod.getAnnotation(SlowTest.class); // 리플렉션을 활용한 방법
    
    if( ... && annotation == null) // 어노테이션도 없을 경우 권장 메시지 출력
  ```
  ![image](https://user-images.githubusercontent.com/76584547/121802420-204ca300-cc77-11eb-89e8-1dd145be98bb.png)
 
 
  + 프로그래밍적 방법
  ```
    생성자로 직접 등록 (@RegisterExtension)
  ```
  + THRESHOLD 를 유동적으로 사용하기 위해 생성자 등록
  ![image](https://user-images.githubusercontent.com/76584547/121802811-57bc4f00-cc79-11eb-8851-88b7746b036b.png)
  
  <BR/>
  + @RegisterExtension를 사용하여 생성자 등록 (THRESHOLD : 유동적 처리 가능)
  
  <BR/>  
  
  ![image](https://user-images.githubusercontent.com/76584547/121802869-a9fd7000-cc79-11eb-86b0-f459d291a233.png)

  
  
  ### Junit5 마이그레이션
  ```
    Junit5 환경에서 Junit5이하의 환경을 사용하고 싶을 때 사용
  ```
  
  + gradle 추가 (vintage, 사용할 버전)
  ![image](https://user-images.githubusercontent.com/76584547/121803335-019cdb00-cc7c-11eb-9579-e4ea3f793115.png)
  <br/>

  + junit4 테스트 작성 (jupiter가 아닌 junit 아래의 메소드들)
  ![image](https://user-images.githubusercontent.com/76584547/121803356-1aa58c00-cc7c-11eb-9949-33eec97f08d6.png)
  <br/>
  
  + 실행한 엔진에 따라 실행한 것을 확인할 수 있다 (Jupiter : 5, Vintage : 4)
  ![image](https://user-images.githubusercontent.com/76584547/121803377-35780080-cc7c-11eb-8467-41e64db7d9b8.png)




  

### 테스트 인스턴스
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
  + junit5부터는 메소드 순서대로 테스트가 실행되긴 하지만 100%는 아니다 내부 로직에 따라 다를 수 있다.
  + @TestMethodOrder(MethodOrderer.OrderAnnotation.class) , @Order(순서) 사용


  ![image](https://user-images.githubusercontent.com/76584547/121009215-db37f500-c7ce-11eb-9d9a-1066ff58311a.png)

  + 결과


  ![image](https://user-images.githubusercontent.com/76584547/121009283-eee35b80-c7ce-11eb-8435-eab4f6e68a23.png)

  + 한 인스턴스로 상태를 공유할 때 사용할 듯하다 

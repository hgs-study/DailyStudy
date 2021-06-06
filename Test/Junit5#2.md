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

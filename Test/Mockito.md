## Mockito

### Mockito란?
-----
```
  Mock을 지원하는 Frwamework
  - Mock : 진짜 객체와 비슷하게 동작하지만 프로그래머가 직접 그 객체의 행동을 관리하는 객체.
  - Mockito : Mock 개겣를 쉽게 만들고 관리하고 검증할 수 있는 방법을 제공한다.
```
<br/>

+ 활용
```
  ㅇ 기선님은 실제 내부에 있는 클래스를 Mock 테스트하지 않고 외부 api를 활용한 테스트만 Mocking하심
  
  ㅇ ex)bango 라는 payment 관련 회사인데 bango가 운영하는 테스트서버를 활용해서 mock 테스트함
  
  ㅇ 기선님은 studyController , Service, Repository를 직접 다 만들고 컨트롤할 수 있는데 이걸 굳이 유닛 테스트
     Mock 테스트를 해야하는가?에 대해는 부정적인 견해
     
  ㅇ 기선님은 studyController의 api를 하나의 행위로 보고 다른 묶여있는 service 등 클래스를 다 같이 하나의 묶여있는
     단위 테스트라고 생각하심 이 부분은 논란이 많을 것.
```
<br/>

+ 의존성
```
  - 스프링 부트에서 spring-boot-starter-test를 의존받고 있는데 이 안에 mockito가 포함, junit5도 포함\
  - mockito-core : mockito가 제공하는 기본적인 기능들
  - mockito-junit-jupiter : junit 테스트에서 mockito를 연동해서 사용할 수 있는 구현체
```
<br/>

+ Mock 활용 방법
```
  - Mock을 만드는 방법
  - Mock이 어떻게 동작해야 하는지 관리하는 방법
  - Mock의 행동을 검증하는 방법
```
<br/>

+ Mock을 사용하기 좋은 경우
```
  의존 받는 것이 인터페이스만 있고 구현체는 없는 경우
```
<br/>

+ Mock 객체 생성#1
```
  Mockito.mock() 메소드를 활용하면 인자로 받는 해당 클래스의 목 객체를 생성해준다.
```
![image](https://user-images.githubusercontent.com/76584547/122667873-26e79700-d1f0-11eb-8867-891b7cd45ea8.png)


+ Mock 객체 생성#2 (Mock 어노테이션 활용)
```
  - @Mock과 @ExtendWith(MockitoExtension.class)를 활용하여 객체 생성
  - @Mock만 사용한다고 만들어주지 않는다. null값이 들어가고 @ExtendWith(MockitoExtension.class)를 함께 사용해야함
```
![image](https://user-images.githubusercontent.com/76584547/122667899-51395480-d1f0-11eb-8c57-f951e4d8beed.png)
<br/>

+ Mock 객체 생성#3 (파라미터에 삽입)
```
  @Mock 어노테이션을 직접 파라미터에 넣어도 만들어준다.
```
![image](https://user-images.githubusercontent.com/76584547/122668232-0fa9a900-d1f2-11eb-88ad-abcfe216f04d.png)

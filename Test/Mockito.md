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

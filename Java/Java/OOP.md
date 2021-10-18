## OOP

#### 객체
---
```
  "기능"으로 정의한 클래스
```

+ 객체일까?
```JAVA
  public class Member{
    private String id;
    private String name;
    
    public void setId(String id){
      this.id = id;
    }
    public String getId(){
      return id;
    }
    public void setName(String name){
      this.name = name;
    }
    public String getName(String name){
      return name;
    }
  }
```
+ 해당 클래스는 데이터(name,id)를 접근하고 조회하기만 하기 때문에 "데이터"클래스라고 불림
+ "기능"이 없으므로 객체보단 데이터 클래스


#### 캡슐화
---
```
  - 객체가 기능 구현을 어떻게 했는 지 외부에 감추는 것
  - 캡슐화만 잘해도 좋은 코드를 만들 가능성이 높아진다
```

+ 절차지향
```java
  if(acc.getMembership() == REGULAR){
    ..정회원 기능
  }
```

+ 캡슐화
```JAVA
  if(acc.hasRegularPermission()){
    ..정회원 기능
  }
```


+ 중요
```
  절차지향일 경우 많은 코드를 바꿔야하는데, 캡슐화를 하면 연쇄적인 변경 전파를 최소화할 수 있다.
```

+ 캡슐화 규칙
  + Tell, Don't Ask
    + 데이터 달라하지 말고 해달라고 하기
    ```java
      acc.getMembership() == REGULAR  => acc.hasRegularPermission()
    ```
  + Demeter's Law
    + 메서드 하나로 호출하라는 법칙
    ```java
      acc.getExpDate().isAfter(now) => acc.isExpired(), acc.isValid(now)
    ```
    

#### 추상화
---
```
  - 여러 구현 클래스를 대표하는 상위 타입 도출 (상위 클래스 <-> 콘트리트 클래스)
  - 공통 기능을 추상화하여 유연함이 증가
```

  + 추상화 시점
  ```
    - 무턱대고 할 경우 추상 타입이 증가하고 복잡도가 증가한다.
    - 실제 변경/확장이 일어날 때 추상화를 시도한다.
  ```

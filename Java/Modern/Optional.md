## Optional
----
```
  - null을 가질 수 있는 선택형 값을 캡슐화하는 클래스
```
  + 특징
    + 값을 감쌀 수 있는 클래스
    + 값의 존재 유무로 다양한 반환값을 가질 수 있다
    + filter와 flatMap 등 다양한 메소드를 포함하고 있다.

![image](https://user-images.githubusercontent.com/76584547/123802563-687ded80-d926-11eb-9213-2b909afb0111.png)


## Optional 메소드 

+ 예시 클래스 설정
```java
    public static class Person{
        private String name;
        private int age;
    }
```

### empty() : 빈 Optional
----
```java
  Optional<Person> hyun = Optional.empty();
  
  System.out.println("hyun.getClass().getName() = " + hyun.getClass().getName());
```

+ 타입 확인

<br/>

![image](https://user-images.githubusercontent.com/76584547/123804257-058d5600-d928-11eb-8b41-0095ab8a9767.png)



### of() 와 ofNullable() : Null을 가질 수 없는 Opional과 가질수 있는 Optional
----



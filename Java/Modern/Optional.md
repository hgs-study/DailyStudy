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

![image](https://user-images.githubusercontent.com/76584547/123805856-65383100-d929-11eb-8514-dd1b146ae8cf.png)

### of() : Null을 가질 수 없는 Opional
----

```java
  Optional<Person> ofTest = Optional.of(null);
```

+ 결과

<br/>

![image](https://user-images.githubusercontent.com/76584547/123805253-e216db00-d928-11eb-9705-28d94007447a.png)


### ofNullable() : Null을 가질 수 있는 Optional
-----
```java
  Optional<Person> ofNullableTest = Optional.ofNullable(null);
```

+ 결과

<br/>

![image](https://user-images.githubusercontent.com/76584547/123805978-797c2e00-d929-11eb-9444-c38b3047a768.png)


### flatMap
---
```
  생성된 모든 스트림이 하나의 스트임으로 병합되어 평준화 된다.
```
  + 예제
  ```java
          String getCarInsuranceName = hyun.flatMap(Person::getCar)
                                           .flatMap(Car::getInsurance)
                                           .map(Insurance::getName)
                                           .orElse("Unknown");
  ```
  
<br/>


### Optional 변수에 절대로 null을 할당하지 말 것
----
```
  반환 값으로 null을 사용하는 것이 위험하기 때문에 등장한 것이 Optional입니다. 
  당연히 Optional 객체 대신 null을 반환하는 것은 Optional의 도입 의도와 맞지 않겠죠.
```
+ 나쁜 예
```java
Optional<Person> findById(Long id) {
  // find person from db
  if (result == 0) {
      return null;
  }
}
```

+ 좋은 예
```java
Optional<Person> findById(Long id) {
    // find person from db
    if (result == 0) {
        return Optional.empty();
    }
}
```

### 값이 없는 경우, Optional.orElseThrow()를 통해 명시적으로 예외를 던질 것
----
```
  값이 없는 경우, 기본값을 반환하는 대신 예외를 던져야 하는 경우도 있습니다. 이 경우에는 Optional.orElseThrow()를 사용할 수 있습니다.
```
+ 예시
```java
  findById(4).orElseThrow(() -> new NoSuchElementException("Person not found"));
```

### ifPresent-get은 orElse나 orElseXXX 등으로 대체할 것
---
```
  Optional 객체로부터 값의 유무를 확인한 뒤 값을 사용하는 패턴은 앞에서 소개한 다양한 API들로 대체할 수 있습니다.
```

+ 피해야 하는 예:
```java
  Optional<Person> maybePerson = findById(4);
  if (maybePerson.isPresent()) {
      Person person = maybePerson.get();
      System.out.println(person.getName());
  } else {
      throw new NoPersonFoundException("No person found id maches: " + 4);
  }
```

+ 좋은 예:
```
  Person person = findById(4)
    .orElseThrow(() -> new NoPersonFoundException("No person found id maches: " + 4));
  System.out.println(person.getName());
```

### Optional을 필드의 타입으로 사용하지 말 것
----
```
 개요에서 다뤘듯이, Optional은 반환 타입을 위해 설계된 타입입니다. 
 뿐만 아니라 Serializable도 아니기 때문에 Optional을 (생성자와 세터를 포함한) 메서드의 인자로 
 사용하거나 클래스의 필드로 선언하는 것은 Optional의 도입 의도에 반하는 패턴입니다.
```

+ 나쁜 예:
```java
class Person {
    Optional<String> address;
}

혹은

void printUserName(Optional<Person> maybePerson) {
    // ...
}
```

+ 좋은 예
```java
class Person {
    String address = "";
}

혹은

void printUserName(Person person) {
    // ...
}
```

추가 참고 : https://www.latera.kr/blog/2019-07-02-effective-optional/

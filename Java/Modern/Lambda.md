## 람다 표현식
```
  ㅇ 람다 표현식
  ㅇ 함수형 인터페이스
  ㅇ Function 패키지
  ㅇ 형식 검사, 추론, 제약
  ㅇ 메서드 참조
  ㅇ Predicate 조합
  ㅇ Function 조합
```

### 람다 표현식
```
  = 메서드로 전달할 수 있는 익명 클래스를 단순화한 것
  
  익명
    - 보통의 메서드와 달리 이름이 없으므로 익명이라고 표현
  함수
    - 람다는 메서드처럼 특정 클래스에 종속되지 않으므로 함수라고 부른다.
  전달
    - 메서드 인수로 전달하거나 변수로 저장
  간결성
    - 익명 클래스와 달리 간결한 코드를 구현한다.
```
<br/>

+ 익명 클래스
```java
        Comparator<Person> byHeight = new Comparator<Person>() {
            @Override
            public int compare(Person p1, Person p2) {
                return p1.getHeight().compareTo(p2.getHeight());
            }
        };
```
<br/>

+ 람다 사용

```java
       Comparator<Person> byHeight =
                (Person p1, Person p2)->p1.getHeight().compareTo(p1.getHeight());
```
<br/>

+ 람다 파라미터와 람다 바디


![image](https://user-images.githubusercontent.com/76584547/121229139-93959400-c8c8-11eb-9b7c-793e91dacf83.png)
+ 왼쪽 박스는 람다 파라미터를 나타낸다.
+ 오른쪽 박스는 람다 바디이며 람다의 반환값에 해당하는 표현식이다.

<br/>

+ 람다 문법
```java
        1. (String s)-> s.length()
        
        2. (Person p) -> p.getHeight() > 150
        
        3. (int x, int y) -> {
            System.out.println("result")
            System.out.println(x+y)
           }
           
        4. () -> 42
        
        5. (Person p1, Person p2) -> p1.getHeight().compareTo(p2.getHeight())
```
  + 표현식 스타일과 블록 스타일을 사용할 수 있다.
  
### 함수형 인터페이스
-----
```
  오직 하나의 수창 메서드만 가지고 있는 인터페이스
```

```java
  public interface Predicate<T>{
    boolean test (T t);
  }
```
<br/>

+ @FunctionalInterface
```
  - 함수형 인터페이스임을 가리키는 어노테이션이다.
  - 추상 메서드가 한 개 이상이라면 Multiple non-overriding abstract methods found in interface com.company.Lambda.PersonFunctionalInterface 에러 발생
```

![image](https://user-images.githubusercontent.com/76584547/121233314-829b5180-c8cd-11eb-9962-32ef0646ce78.png)

![image](https://user-images.githubusercontent.com/76584547/121232932-0c96ea80-c8cd-11eb-80df-5dbd17ce4922.png)


### 함수 디스크립터
----
```
  - 람다 표현식의 시그니처를 서술하는 메서드
  - boolean test(T t)가 메서드 시그니처라면 함수 디스크립터로 표현을 하면 T->boolean 이라 할 수 있다.
```

+ Predicate
```
  test라는 추상 메서드를 정의하며 test는 제네릭형식 T의 객체를 인수로 받아 불리언을 반환한다.
```

![image](https://user-images.githubusercontent.com/76584547/121233563-b6767700-c8cd-11eb-9d2f-039fc5299dd5.png)


+ Consumer
```
  제네릭 T 객체를 받아서 void를 반환하는 accept라는 추상 메드를 정의한다.
```

![image](https://user-images.githubusercontent.com/76584547/121233655-cc843780-c8cd-11eb-9562-8ac2f9fc17ab.png)


+ Function
```
  제네릭 형식 T를 받아서 제네릭 형식 R 객체를 반환하는 추상 메서드 apply를 정의한다.
```

![image](https://user-images.githubusercontent.com/76584547/121233773-f2a9d780-c8cd-11eb-8a7b-4e8b247bcf25.png)

### 기본형 특화
----
```
  - Consumer<T>의 T 제네릭 파라미터는 참조형만 사용가능하다
  - 참조형 타입 : Byte, Integer, Object, List
  - 기본형 타입 : int, double, byte, char
```
  + 박싱 : 기본형을 참조형으로 변환하는 기능
  + 언박싱 : 참조형을 기본형으로 변환하는 기능
  + 오토박싱 : 편리하게 코드를 구현할 수 있도록 박싱과 언박싱이 자동으로 이루어진다.

+ 문제점
```
  비용 소모
  - 박싱한 값은 기본형을 감싸는 래퍼이며 힙에 저장된다.
  - 박싱한 값은 메모리를 더 소비하며 기본형을 가져올 때도 메모리를 탐색하는 과정이 필요
```

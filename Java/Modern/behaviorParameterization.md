## 동작 파라미터화
```
  ㅇ 동작 파라미터
  ㅇ 익명 클래스
  ㅇ 람다 표현식 
```

### 동작 파라미터
```
  - 메서드 내부적으로 다양한 동작을 수행할 수 있도록 코드를 메서드 인수로 전달하는 것
```

+ Predicate 
```JAVA
public interface ApplePredicate {
    boolean test(Apple apple);
}
```
+ 프레디케이트(Predicate)란?
  + 참 또는 거짓을 반환하는 함수
  + 선택 조건을 결정하는 인터페이스로 활용할 수 있음
+ [Strategy Design Pattern] - 알고리즘 패밀리
<BR/>

+ Strategy
```JAVA
public class AppleGreenColorPredicate implements ApplePredicate{

    @Override
    public boolean test(Apple apple) {
        return GREEN.equals(apple.getColor());
    }
}

```
```JAVA
public class AppleGreenColorPredicate implements ApplePredicate{

    @Override
    public boolean test(Apple apple) {
        return GREEN.equals(apple.getColor());
    }
}

```
※ 전략 디자인 패턴(Strategy Design Pattern)이란?
  ```
    - 각 알고리즘(전략이라 불리는)을 캡슐화하는 알고리즘 패밀리를 정의해둔 다음에 알고리즘을 선택하는 기법
  ```
  
  ```java
    public interface MovableStrategy {
        public void move();
    }
    --------------------------------------
    public class RailLoadStrategy implements MovableStrategy{
        public void move(){
            System.out.println("선로를 통해 이동");
        }
    }
    --------------------------------------
    public class LoadStrategy implements MovableStrategy{
        public void move() {
            System.out.println("도로를 통해 이동");
        }
    }
  ```
  출처 : https://victorydntmd.tistory.com/292
  
+ [Strategy Design Pattern] - 알고리즘 패밀리, 전략


<BR/>

<BR/>

+ filterApples
```JAVA
    public static List<Apple> filterApples(List<Apple> inventory, ApplePredicate p){
        List<Apple> apples = new ArrayList<>();

        for(Apple apple : inventory){
            if(p.test(apple))
                apples.add(apple);
        }

        return apples;
    }
```
+ 유연하며 가독성이 좋고 사용하기 쉬워진 특징을 가짐


+ 결과

![image](https://user-images.githubusercontent.com/76584547/120346026-f79ee200-c335-11eb-9a00-31a5002e3152.png)
![image](https://user-images.githubusercontent.com/76584547/120346058-ff5e8680-c335-11eb-804a-caaa9f500aa2.png)

<br/>

### 익명 클래스
```
  - 말그대로 이름이 없는 클래스
  - 클래스 선언과 인스턴스화를 동시에 할 수 있음
  - 즉석에서 필요한 구현을 만들어서 사용 가능
  - 일급 객체
```

```java
    public static List<Apple> AnonymousClass( List<Apple> apples){
        return filterApples(apples, new ApplePredicate() {
            @Override
            public boolean test(Apple apple) {
                return RED.equals(apple.getColor());
            }
        });
    }
```

+결과

![image](https://user-images.githubusercontent.com/76584547/120348474-2f0e8e00-c338-11eb-9594-db2964139c37.png)

![image](https://user-images.githubusercontent.com/76584547/120348169-e48d1180-c337-11eb-8b12-4fa976d72a63.png)


### 람다 표현식
```
  - 메서드로 전달할 수 있는 익명함수를 단순화 한 것
```

```java
    public static List<Apple> Lambda( List<Apple> apples){
        return filterApples(apples, (Apple apple)-> ORANGE.equals(apple.getColor()));
    }
```

+ 결과

![image](https://user-images.githubusercontent.com/76584547/120349682-44d08300-c339-11eb-88b9-06f80cdb29bb.png)

![image](https://user-images.githubusercontent.com/76584547/120349701-49953700-c339-11eb-9768-ab24b6f1e73f.png)

![image](https://user-images.githubusercontent.com/76584547/120349725-5154db80-c339-11eb-9c5a-6915a5d2ff6c.png)





## 동작 파라미터화
```
  ㅇ Predicate
  ㅇ Strategy Design Pattern
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

+ Strategy-1
```JAVA
public class AppleGreenColorPredicate implements ApplePredicate{

    @Override
    public boolean test(Apple apple) {
        return GREEN.equals(apple.getColor());
    }
}

```
+ Strategy-2
```JAVA
public class AppleGreenColorPredicate implements ApplePredicate{

    @Override
    public boolean test(Apple apple) {
        return GREEN.equals(apple.getColor());
    }
}

```
+ 전략 디자인 패턴(Strategy Design Pattern)이란?
  ```
    - 각 알고리즘(전략이라 불리는)을 캡슐화하는 알고리즘 패밀리를 정의해둔 다음에 알고리즘을 선택하는 기법
  ```
  
  ```java
    public interface MovableStrategy {
        public void move();
    }
    -------
    public class RailLoadStrategy implements MovableStrategy{
        public void move(){
            System.out.println("선로를 통해 이동");
        }
    }
    --------
    public class LoadStrategy implements MovableStrategy{
        public void move() {
            System.out.println("도로를 통해 이동");
        }
    }
  ```
+ [Strategy Design Pattern] - 알고리즘 패밀리


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

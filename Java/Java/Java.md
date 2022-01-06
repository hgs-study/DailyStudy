## Java


#### JVM 구조
---
+ 참고
```
  https://velog.io/@litien/JVM-%EA%B5%AC%EC%A1%B0
```

#### GC
---

+ GC 과정
  + GC의 과정을 Mark and Sweep이라고도 한다. 
  + GC가 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 각각 어떤 객체를 참조하고 있는지 찾는 과정이 Mark라고 한다. 
  + 이 과정에서 Stop the world가 발생한다. 
  + 이후 Mark 되어있지 않은 객체들을 힙에서 제거하는 과정이 Sweep이다.


+ 참고
```
  https://velog.io/@litien/%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%ED%84%B0GC
```


#### 프로세스 vs 쓰레드
---
+ 차이
```
  ㅇ 프로세스 : 프로세스는 운영체제로부터 자원을 할당 받는 작업 단위
  ㅇ 쓰레드 : 할당받은 자원을 이용하는 실행 단위
  
  ㅇ 각 프로세스는 데이터, 코드 , 힙, 스택을 가지고 있지만
  ㅇ 각 쓰레드는 한 프로세스의 데이터, 코드, 힙을 공유받으며 각각의 스택을 가지고있다.
```

#### List
---
+ ArrayList vs LinkedList
```
ㅇ ArrayList : 
- 자바의 Vector를 개선한, 배열로 구현된 List입니다.
- 배열과 같은 자료구조이기 때문에, 리스트의 연산 수행시간 속도는 배열과 같습니다.

ㅇ LinkedList
- 다음 노드의 주소를 기억하고 있는 List로, 배열에 비해 삽입과 삭제가 간단하다
- 하지만 탐색의 경우 첫번째 노드부터 탐색해나가야하기 때문에 느리다는 단점
```

+ Array vs ArrayList
```
  ㅇ Array
  - 크기가 고정되어 있다
  - int, byte, char 등 프리미티브 타입과 오브젝트를 모두 담을 수 있다.
  
  ㅇ ArrayList
  - 사이즈가 동적인 배열 (크기 고정 x)
  - 오브젝트만 담을 수 있다.
```

#### String , intern()
---
+ 참고
```
  https://simple-ing.tistory.com/3
```


#### 변수
---
``` 
  1. 클래스 변수 (클래스 영역) : 클래스가 메모리에 올라갈 때 생성
  2. 인스턴스 변수 (클래스 영역) : 인스턴스가 생성될 떄
  3. 지역 변수 (메서드,생성자,초기화 블럭) : 변수 선언문이 수행 되었을 때
```

#### 추상클래스
---
``` 
  1. 추상 메서드 외에 멤버 변수나 일반 메서드를 가질 수 있다.
  2. 추상 클래스를 설계하여 상속 받는 곳에서 구현한다.
```

#### 인터페이스
---
``` 
  1. 자바에선 다중 상속을 지원하지 않지만 인터페이스는 다중 상속이 가능하며 추상 메서드와 상수만을 가질 수 있다.(추상 클래스와 대비)
  2. 추상클래스보다 더 추상화 정도가 높다. 설계만 가능하고 구현은 상속받는 곳에서 한다.
  3. 팀 프로젝트시에 동시에 사용할 수 있어서 더 많이 쓰인다.
  4. 다중 상속 구현 시 겹치는 메소드가 있으면 하나만 구현해주면 된다.
```

#### 응집도
---
``` 
  - 일반적으로 메서드가 변수를 더 많이 사용할수록 메서드와 클래스는 응집도가 높다.
  - 모든 인스턴스 변수를 메서드마다 사용하는 클래스는 응집도가 높다. (★)
  - 응집도가 높다는 말은 클래스에 속한 메서드와 변수가 서로 의존하며 논리적인 단위로 묶인다는 의미이기 때문이다.
  - "함수는 작게, 매개변수 목록을 짧게"라는 전략을 따르다보면 인스턴스 변수가 아주 많아진다.
      -> 이는 곧 변수와 메서드를 적절히 분리해 새로운 클래스 두 세 개로 쪼개준다.
```

#### 결합도
---
```
  - 클래스간에 얼마나 연결이 되어있는지 나타내는 것이다.
  - 프로젝트가 커지면 커질수록 클래스간 연결이 많다면 하나의 수정을 위해 많은 클래스들을 수정해야할 것
  - 결합도가 높은 프로젝트는 유지보수가 어렵고 시간이 많이 들기에 그만큼 비용소모도 커진다.
```


#### 인터페이스
---
``` 
  1. 자바에선 다중 상속을 지원하지 않지만 인터페이스는 다중 상속이 가능하며 추상 메서드와 상수만을 가질 수 있다.(추상 클래스와 대비)
  2. 추상클래스보다 더 추상화 정도가 높다. 설계만 가능하고 구현은 상속받는 곳에서 한다.
  3. 팀 프로젝트시에 동시에 사용할 수 있어서 더 많이 쓰인다.
  4. 다중 상속 구현 시 겹치는 메소드가 있으면 하나만 구현해주면 된다.
```


#### Json 파싱
---
```
  참고 : https://codechacha.com/ko/java-parse-json/
```

#### 멀티 쓰레드 환경에서 동시성 제어
---
```
  참고 : https://deveric.tistory.com/104
```

#### 블럭, 논블럭, 동기, 비동기
---
  + 블럭 / 논블럭
  ```
    - 블럭/논블럭은 함수호출에서의 이야기이다.(기술적으로 명확히 구분된다.) 
    - A 라는 함수를 호출했을때, A라는 함수를 호출 했을 때 기대하는 행위를 모두 끝마칠때까지 기다렸다가 리턴되면, 이것은 블로킹 되었다고 한다.
    - A 라는 함수를 호출 했는데, A라는 함수를 호출 했을 때 기대하는 어떤 행위를  요청 하고 바로 리턴되면 이것은 논블럭킹 되었다고 한다.
  ```
  
  + 동기 / 비동기
  ```
    - 동기/비동기는 행위에 대한 이야기이다.(기술적으로 구분 안된다. 추상적으로 구분한다.) 
    - A 라는 행위와 B 라는 별개의 행위가 있다고 하자. A 라는 행위와 B 라는 행위가 동시(or 순차적이지 않다면)에 실행되고 있으면 비동기라고 한다. 여기서 제약이 하나 있는데 A,B 행위 사이에는 인과       관계가 있어야 한다. 즉 웹서버를 예로 들어서 멀티쓰레드로 각각 A와B가 다른 클라이언트와 작업 할 때 둘은 동시에 작업하고 있지만, 둘의 인과관계는 없지 않나? 이땐 비동기라고 볼 수 없다. 
    - A라는 행위와  B라는 행위가 순차적으로 작동한다면 "동기"라고 한다. 
  ```
  
  + 참고
  ```
    참고 : https://hamait.tistory.com/930
  ```

  + CompletableFuture
  ```
    - https://brunch.co.kr/@springboot/267
    - https://duooo-story.tistory.com/39
  ```

#### 공변, 반공변
---
+ 공변
```
  A가 B의 상위 타입이고 T<A>가 T<B>의 상위 타입이면 공변
```

```java
  public class Animal {}
  public class Carnivore extends Animal {}
  public class Tiger extends Carnivore {}
  public class Lion extends Carnivore {}
  
  -----

  public class Zookeeper{
    public void giveMeat{
      Cage<? extends Carnivore> cage,
      Meat meat){
      List<Carnivores> cs = cage.getAll9();
      ...
    }
  }
  
  -----------
  
  Zookeeper zk = new Zookeeper();
  
  //Cage<? extends Carnivore> 타입에
  //Cage<Tiger> 할당 가능
  Cage<Tiger> ct = new Cage<>();
  zk.giveMeat(ct, someMeat);
  
  //Cage<? extends Carnivore> 타입에
  //Cage<Lion> 할당 가능
  Cage<Lion> cl = new Cage<>();
  zk.giveMeat(cl, someMeat);
```

+ 하지만 공변에서 지네릭 타입을 사용하는 메서드에 값 전달 안 됨
```java
  Cage<Tiger> ct = new Cage<Tiger>();
  
  Cage<? extends Carnivore> cage = ct;
  cage.push(new Tiger()); // 에러
  // push(? extends Carnivore)
  // cage의 실제 타입이
  // Cage<Tiger>인지 Cage<Lion>인지 알 수 없음
  
  -------
  public class Cage<T>{
    public void push(T animal){
      ...
    }
  }
```

+ 반공변
```
  A가 B의 상위 타입이고 T<A>가 T<B>의 하위 타입이면 반공변
```

```JAVA
  Cage<Tiger> ct = new Cage<>();
  //Cage<? super Tiger> 타입에 Cage<Tiger> 할당 가능
  Cage<? super Tiger> ctt = ct;
  ctt.push(new Tiger()); //OK, ctt는 최소 Cage<Tiger>나 그 상위 타입
  
  Cage<Tiger> ct2 = new Cage<>();
  //Cage<? super Tiger> 타입에 Cage<Tiger> 할당 가능
  Cage<? super Tiger> ctt2 = ct2;
  ctt2.push(new Tiger()); //OK, ctt는 최소 Cage<Tiger>나 그 상위 타입
```

+ PECS
```
  producer - extends
  consumer - super
  => 값을 제공하면 (getAll()) extends, 
  => 값을 사용하면 (push()) super
```

+ NIO
```
  https://sightstudio.tistory.com/15
```

+ Thread Safe, Multi Thread
```
  https://alwayspr.tistory.com/11
```

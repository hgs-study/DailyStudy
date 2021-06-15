## Stream
----
```
  데이터 처리 연산을 지원하도록 소스에서 추출된 연속된 요소
```
  + 특징
    + 선언형 : 더 간결하고 가독성이 좋아진다.
    + 조립할 수 있음 : 유연성이 좋아진다.
    + 병렬화 : 성능이 좋아진다.


### 컬렉션 vs 스트림
-----
```
  공통점 : 컬렉션과 스트림 모두 연속된 요소 형식의 값을 저장하는 자료구조의 인터페이스를 제공
  
  차이점 : 데이터를 언제 계산하느냐가 가장 큰 차이
```

  + 컬렉션
    + 현재 자료구조가 포함하는 모든 값을 메모리에 저장하는 구조 
    + 컬렉션의 모든 요소를 메모리에 저장해야 하며 컬렉션에 추가하려는 요소ㅓ는 미리 계산되어야한다. 
    + 적극적 생성 : 모든 값을 계산할 때까지 기다림
    + 외부 반복 : 명시적으로 컬렉션 항목을 하나씩 가져와서 처리
  
  + 스트림
    + 요청할 때만 요소를 계산하는 고정된 자료구조 
    + 사용자가 요청하는 값만 스트림에서 추출하는 것이 핵심
    + 게으른 생성 : 필요할 때만 값을 계산
    
    
  ```
  ㅇ외부 반복 vs 내부 반복
  
  외부 반복(컬렉션)
    - 명시적으로 컬렉션 항목을 하나씩 가져와서 처리
    - 병렬성 스스로 관리해야한다
    
  내부 반복(스트림)
    - 반복을 알아서 처리해주고 결과값을 어딘가에 저장해준다
    - 투명하게 병렬로 처리하거나 더 최적화된 다양한 순서로 처리
       
  ```
  
  
### 스트림 연산
----
```
  ㅇ 중간연산
  ㅇ 최종연산
```

+ 중간 연산
```
  - filter, map, limit 등 서로 연결되어 다른 스트림을 연결
  - 게으름, 단말 연산을 스트림 파이프라인에 실행하기 전까지는 아무 연산도 수행하지 않는다.
```

+ 최종 연산
```
  - 스트림 파이프 라인에서 결과를 도출한다.
  - 보통 List, Integer, void 등 스트림 이외의 결과가 반환
```


### 스트림 필터링 & 슬라이싱
------
+ filter : 일치하는 요소 반환
```java
        //filter 프레디케이트
        List<Dish> vegetarianMenu = menu.stream()
                                        .filter(Dish::isVegetarian)
                                        .collect(Collectors.toList());
```
  
+ distinct : 중복 제거
```java
        //고유 요소 필터링 (distinct)
        List<Dish> distinctMenu = menu.stream()
                                        .filter(d -> d.getName().equals("pizza"))
                                        .distinct()
                                        .collect(Collectors.toList());
```

+ takewhile : 조건에 맞지 않는 데이터가 나올 경우 중단 (자바 9부터 지원)
```java
        // 조건에 맞지 않는 데이터가 나올 경우 중단 (takeWhile)
        List<Dish> takewhileMenu = menu.stream()
                                        .takeWhile( d-> d.getCalories() < 320)
                                        .collect(Collectors.toList());
```


+ dropWhile : 거짓이 되는 요소가 발견되면 중단하고 남은 요소 반환(자바 9부터 지원)
```java
        // 거짓이 되는 요소가 발견되면 중단하고 남은 요소 반환 (dropWhile)
        List<Dish> dropWhileMenu = menu.stream()
                                        .dropWhile( d-> d.getCalories() < 320)
                                        .collect(Collectors.toList());
```


+ limit : 주어진 값 이하의 크기를 갖는 새로운 스트림 반환
```java
        //스트림 축소 (limit)
        List<Dish> vegetarianLimitMenu = menu.stream()
                                        .filter(Dish::isVegetarian)
                                        .limit(3)
                                        .collect(Collectors.toList());
```


+ skip : 처음 n개의 요소를 제외한 스트림을 반환
```java
        //요소 건너띄기 (skip)
        List<Dish> skipMenu = menu.stream()
                                        .filter(d-> d.getCalories() < 320)
                                        .limit(2)
                                        .collect(Collectors.toList());
```

### 스트림 매핑
-----
```
  특정 데이터를 선택하고 새로운 요소를 매핑해서 새로운 버전을 만듦
```
+ map
```java
        // 요리명을 다시 길이로 매핑
        List<Integer> dishNameLengths = menu.stream()
                                            .map(Dish::getName)
                                            .map(String::length)
                                            .collect(Collectors.toList());
```

+ flatMap : 스트림의 각 값을 다른 스트림으로 만든 다음에 모든 스트림을 하나의 스트림으로 만드는 평면화
```java
        List<String> uniqueCharacters = words.stream()
                                             .map(word -> word.split(""))
                                             .flatMap(Arrays::stream)
                                             .distinct()
                                             .collect(Collectors.toList());
```
![image](https://user-images.githubusercontent.com/76584547/122053638-e370df80-ce21-11eb-9848-3920ca551a4a.png)

<br/>

### 스트림 검색
---
```
  - 특정 속성이 데이터 집합에 있는지 여부를 검색
  - allMatch, anyMatch, noneMatch, findFirst, findAny
```

+ allMatch : 모든 요소가 주어진 프레디케이트와 일치하는지 검사 <-> noneMatch : 요소 없는지 검사
```java
        //allMatch
        boolean isHealthy = menu.stream()
                                .allMatch(d -> d.getCalories() <1000);
```

+ 쇼트서킷
```
  - anyMatch, allMatch, noneMatch는 쇼트서킷 기법 + limit
  - 표현식에서 하나라도 거짓이라는 결과가 나오면 나머지 표현식의 결과와 상관없이 전체 결과도 거짓이 된다.
```

+ findAny : 현재 스트림에서 임의의 요소를 반환
+ findFirst : 첫번째 요소 찾기

### 리듀싱
----
```
  모든 스트림의 요소를 처리해서 값으로 도출하는 것 (=폴드)
```  

+ reduce : 스트림 요소의 합, 초깃값 : 0
```java
        //reduce 스트림 요소의 합 , 초깃값 : 0
        int sum = numbers.stream() 
                         .reduce(0, (a,b) -> a + b);
```

###  스트림 연산 : 상태 없음과 상태 있음
----
```
  ㅇ 상태를 갖지 않는 연산 : filter, map
  ㅇ 상태를 갖고 있는 연산 : reduce, sum, max (크기 한정)
  ㅇ 상태 o, 모든 요소 버퍼에 추가 : sorted, distinct (과거 이력을 알고 있어야함) 
```

### 기본형 특화 스트림
----
+ 박싱 비용을 가지고 있다.
```java
        int sum = numbers.stream()
                         .reduce(0, (a,b) -> a + b);
```
<br/>


+ 언박싱 stream을 박싱 stream으로 변환
```java
        IntStream intStream = menu.stream().mapToInt(Dish::getCalories);
        Stream<Integer> integerStream = intStream.boxed();
```

### 숫자 범위
----
+ range(x,y) : x,y 제외
+ rangeClosed(x,y) : x,y 포함 결과
```java
        //숫자 범위
        IntStream evenNumber = IntStream.rangeClosed(1,100)
                                        .filter(n -> n % 2 ==0);
```

### 값으로 스트림 만들기
---
+ of
```java
        Stream<String> stringStream = Stream.of("Modern" , "java", "Name" , "Hyun");
        stringStream.map(String::toUpperCase)
                    .forEach(System.out::println);
```

+ empty 
```java
        //스트림 비우기
        Stream<String> emptyStream = Stream.empty();
```

### 무한 스트림
```
  ㅇ iterate 
  ㅇ generate
```

+ iterate
```
  - 크기가 고정되지 않은 무한 스트림을 만들 수 있다.
  - 요청할 때마다 끝 없이 생산 (=언바운드 스트림)
  - 스트림과 컬렉션의 큰 차이점
```
+ 예제
```java
        //무한 스트림 (iterate)
        Stream.iterate( 0, m-> m +3)
              .limit(10)
              .forEach(System.out::println);  
```

+ generate
```
  - iterate와 달리 값을 연속적으로 계산하지 않는다.
```
+ 예제
```java
        //무한 스트림 (generate)
        Stream.generate(Math::random)
              .limit(5)
              .forEach(System.out::println);
```

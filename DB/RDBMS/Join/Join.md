## Join
----
```
  ㅇ Nested Loop Join
  ㅇ Sort Merge Join
  ㅇ Hash Join
```

### Nested Loop Join (NL Join)
-----
+ Nested Loop Join
  + 선행 테이블(Outer Input Table) A의 조건 검색 결과값을 하나하나를 후행 테이블(Inner Input Table) B와 비교하여 조인하는 방식
  + 이중 for문과 흡사한 구조이다.
  + "단일 코어"로만 데이터를 처리하기 때문에, Scale-Out이 아닌 "Scale-Up"이 효율적이다.
  + 데이터를 처리하면 매번 데이터 접근 요청을 하기 때문에 매우 비효율적이다
  + 이를 개선한게 조인 버퍼를 이용한 "Block Nested Loop Join"이다.

+ 선행 테이블(dept)의 조건을 먼저 검사한 후 조인컬럼의 emp의 dept_no 인덱스 참조 후 59000을 만족하는 데이터만 최종 결과가 된다.
  + 선행테이블의 처리 범위만큼 후행테이블의 랜덤 액세스를 시도하고 테이블 액세스 시 인덱스를 찾지 못하면 매번 풀스캔을 해야한다.
![image](https://user-images.githubusercontent.com/76584547/127828257-36c56b0b-723d-47f8-a574-8b81500f6d25.png)


#### Block Nested Loop Join ("조인 버퍼" 도입)
 + Block Nested Loop Join
    + 테이블 조인 시 필요한 데이터를 메모리에 일시적으로 저장하여 효율적으로 데이터에 접근
    + STEP1. 테이블 A의 조인 대상을 찾는다.
    + STEP2. 조인할 데이터를 [조인 버퍼]가 가득 찰 때까지 채운다.
    + STEP3. 테이블 B에 테이블 풀스캔, 인덱스 풀스캔, 인덱스 범위 스캔 등으로 데이터 접근한다.
    + 여기서 테이블 B의 스캔하는 횟수는 "조인 버퍼에 데이터가 적재되는 횟수"와 동일하다.


### Sort Merge Join
-----
```
 조인 키를 모두 정렬한 후에 조인을 수행하는 특징이 있지만, 조인 작업을 위해 항상 정렬 작업이 발생하는 것은 아니다.
```
+ 특징
  + 이미 앞 단계에서 작업을 수행하는 도중 정렬작업이 미리 되어있다면 조인을 위한 정렬 작업은 발생하지 않을 수 있다.
  + 정렬한 데이터가 많아 메모리에서 모든 정렬 작업을 수행하기 어려운 경우 임시 영역(디스크)을 사용하기 때문에 성능이 떨어질 수 있다.
  + 그래서 일반적으로 대량의 조인 작업에서 정렬 작업을 필요로하는 Sort Merge Join 보다는 "CPU 작업 위주"로 처리하는 "Hash Join"이 성능상 유리하다.

![image](https://user-images.githubusercontent.com/76584547/127831609-fb44bbb9-c522-4c93-a842-d33ff18959a7.png)

<br/>

+ dept_no으로 정렬하고 dept_no이 90미만인 것을 SORT로 가져온 후, 결과 값을 dept_no 순으로 정렬한다.
+ dept_no으로 정렬하고 E.sal이 59000것을 가져와서 dept_no 순으로 정렬한다.
![image](https://user-images.githubusercontent.com/76584547/127831658-f31fffb1-0a08-4ec1-8218-2542d681dcdc.png)


### Hash Join
----
```
  해쉬 테이블을 생성하여 조인하는 방식
```
![image](https://user-images.githubusercontent.com/76584547/127835439-28705c82-47f0-4f1d-8476-d693994d0a89.png)

+ 특징
  + 선행 테이블에서 dept_no이 90미만은 데이터에 해쉬 펑션을 적용하여 "해쉬 테이블을 생성"하게 된다. 이 작업을 모든 행위에 대해 "반복 수행"한다.
  + 후행 테이블은 해쉬 테이블의 해쉬 밸류를 검사하며, 샐러리가 59000원 초과하는 행을 찾는다.
  + 그리고 후행 테이블의 조인키를 기준으로 해시 함수를 적용하여 해시 테이블 내에 동일한 데이터를 찾는 것
  + 매칭이 되면 운반 단위로 보내게 된다.
  + 해시 테이블은 조건키(dept_no)으로 찾기 때문에 등치조건(D.dept_no = E.dept_no)이 반드시 필요하다.

![image](https://user-images.githubusercontent.com/76584547/127835798-dd1267fb-a308-4953-a383-3a0eba15b7ea.png)


#### 참고
----
1. https://hoon93.tistory.com/46
2. https://www.youtube.com/watch?v=Lq9BVruQMKg

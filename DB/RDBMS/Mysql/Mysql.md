
### 서버엔진
-------
```
클라이언트의 요청을 받아 SQL을 처리하는 DB 자체의 기능적인 역할을 담당한다.
```
+ 쿼리 파싱
   + 사용자가 쿼리를 날렸을ㅇ 때, DB가 SQL을 이해할 수 있도록 쿼리를 재구성하는 것
+ 데이터 요청
   + 디스크나 메모리 같은 물리적인 저장장치와 통신하는 스토리지에서 데이터 요청 업무 담당
+ 데이터 처리
   + 스토리지 엔진에서 가져온 데이터를 처리하는 역할
   + Table Join, Group By, Order By와 같은 일반적인 SQL 처리부터
   + Function / Procedure, Trigger, Constraint 등과 같은 기능도 처리  


### 스토리지 엔진
-------
```
서버엔진이 필요한 데이터를 물리 장치에서 가져오는 역할
```
+ Mysql은 다른 DBMS와 다르게 스토리지 엔진이 플러그인 방식으로 동작한다.
   + Mysql 5.5 버전에선 스토리지 엔진 8개가 기본적으로 설치되어 있다.
   ```
   Inno DB, MyISAM, MRG_MYISAM, BLACKHOLE, CSV, MEMORY, FEDERATED, ARCHIVE
   ```
   + Mysql은 다른 상용 DBMS와 다르게 DB엔진에 완벽하게 최적화되어 있지 않다.
   ```
   - 이는 다양한 스토리지 엔진에서 동작할 수 있어야 하기 때문이다. 
   - 그래서 데이터 처리가 일부 비효율적일 수도 있지만 확장성 면에서는 다른 어떠한 DBMS보다 유연하다.
   ```
+ Mysql은 강력한 특징 중 하나는 바로 스토리지 엔진의 다양성이다.
+ 주요 스토리지 엔진 MyISAM, Inno DB, Archive 차이
   + MyISAM
   ```
   - 조회 시 빠른 속도 (풀 텍스트 인덱스 지원)
   - 카운트, 집계(함수) 빠름
   - Table Level Locking
   - Select, Insert, Delete, Update 시 해당 Table 전체 Lock이 걸린다.
   - row 수가 늘어날 수록 엄청 느려진다.
   - 인덱스만 메모리에 올린다.
   ```
   + Inno DB
   ```
   - Order By 압도적으로 빠르다.
   - 대용량 처리 빠름
   - Row Level Locking
   - 인덱스 + 데이터를 모두 메모리에 올린다. (메모리 버퍼 크기 (InnoDB_Buffer_Pool_Size)가 DB 성능에 큰 영향을 미친다.)
   ```

### JSON 컬럼
```
 - json_set : 하나 이상의 key값을 변경하고자 할때, json_set 함수를 사용
 - json_array : 여러 개의 value를 배열로 만든다. ["value1","value2","value3"] 이런 식으로 들어간다.

```



+ json_set : 하나 이상의 key값을 변경하고자 할때, json_set 함수를 사용
```
MariaDB [test]> update json_test set data = json_set(data,'$.Phone','333-4444', '$.Name','DongHyeok KIM') where id = 1 ;

Query OK, 1 row affected (0.12 sec)

Rows matched: 1  Changed: 1  Warnings: 0

 

MariaDB [test]> commit ;

Query OK, 0 rows affected (0.00 sec)

 

MariaDB [test]> select * from json_test ;

+------+------------------------------------------------------------+

| id   | data                                                       |

+------+------------------------------------------------------------+

|    1 | {"Name": "DongHyeok KIM", "Sex": "M", "Phone": "333-4444"} |

+------+------------------------------------------------------------+

1 row in set (0.00 sec)

 

MariaDB [test]> select id , json_value(data,'$.Name') As Name , json_value(data,'$.Phone') as Phone from json_test ;

+------+---------------+----------+

| id   | Name          | Phone    |

+------+---------------+----------+

|    1 | DongHyeok KIM | 333-4444 |

+------+---------------+----------+

1 row in set (0.00 sec)

 

MariaDB [test]> insert into json_test values (2 , json_object('Name' , 'Kildong HONG' , 'Sex' , 'M' , 'Phone' , '999-9999' , 'Birth', '2000-01-01')) ;

Query OK, 1 row affected (0.09 sec)

 

MariaDB [test]> commit ;

Query OK, 0 rows affected (0.00 sec)

 

MariaDB [test]> select * from json_test ;

+------+----------------------------------------------------------------------------------+

| id   | data                                                                             |

+------+----------------------------------------------------------------------------------+

|    1 | {"Name": "DongHyeok KIM", "Sex": "M", "Phone": "333-4444"}                       |

|    2 | {"Name": "Kildong HONG", "Sex": "M", "Phone": "999-9999", "Birth": "2000-01-01"} |

+------+----------------------------------------------------------------------------------+

2 rows in set (0.00 sec)



출처: https://bstar36.tistory.com/359 [멋지게 놀아라]
```


+ json_array : 여러 개의 value를 배열로 만든다. ["value1","value2","value3"] 이런 식으로 들어간다.
```
Mysql에서 3,4,5,6,7,8을 json의 배열로 넣고 싶을경우

sql : json_array("3","4","5","6","7","8")  

결과 : ["3","4","5","6","7","8"]

```

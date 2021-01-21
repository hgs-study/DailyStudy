## JSON 컬럼
```
 - json_set : 하나 이상의 key값을 변경하고자 할때, json_set 함수를 사용

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

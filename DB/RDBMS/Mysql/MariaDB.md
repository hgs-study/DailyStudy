## MariaDB
```
  ㅇ 설치 (AWS)
```

ㅇ 설치 (AWS - Ubuntu)
  + 참고 : https://mirmond.tistory.com/entry/%EC%9A%B0%EB%B6%84%ED%88%AC-mariadb-103-%EC%84%A4%EC%B9%98
```
  apt-get -y install software-properties-common dirmngr
  apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xF1656F24C74CD1D8
  add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://mirror.zol.co.zw/mariadb/repo/10.3/debian stretch main'
  apt-get update
  apt-get install -y mariadb-server-10.3
  apt-get install mysql-client
```

ㅇ 시간 변경
```
  mysql -u root -p
  SELECT NOW();
  sudo timedatectl set-timezone 'Asia/Seoul';
  sudo systemctl restart mysqld
```

ㅇ 외부 포트 열기
```
  /etc/mysql/my.cnf 
  -> bind-address = 0.0.0.0 으로 변경
```


## MariaDB
```
  ㅇ 설치 (AWS)
```

ㅇ 설치 (AWS - Ubuntu)
  + 참고 : https://mirmond.tistory.com/entry/%EC%9A%B0%EB%B6%84%ED%88%AC-mariadb-103-%EC%84%A4%EC%B9%98
```
  apt-get -y install software-properties-common dirmngr
  apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xF1656F24C74CD1D8
  add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://mirror.zol.co.zw/mariadb/repo/10.3/debian stretch main'
  apt-get update
  apt-get install -y mariadb-server-10.3
  apt-get install mysql-client
```

ㅇ 시간 변경
```
  mysql -u root -p
  SELECT NOW();
  sudo timedatectl set-timezone 'Asia/Seoul';
  sudo systemctl restart mysqld
```

ㅇ 외부 포트 열기
```
  /etc/mysql/my.cnf 
  -> bind-address = 0.0.0.0 으로 변경
```


ㅇ 리눅스에서 mariadb 자동 백업 (crontab) 
```
   - https://code-aid.tistory.com/7 참고
   - sudo crontab -e (관리자 권한으로 실행)
```
+ backup.sh
```sh
#!/bin/sh
 

# 백업 위치를 /backup 아래로 정한다.

# 백업 시간을 년-월-일 형식으로 지정한다.
DATE=`date +"%Y%m%d%H%M%S"`

 

# 사용자 계정과 비밀번호

USERNAME="DB계정"

PASSWORD="DB비밀번호"

 

# 백업할 데이타베이스

DATABASE="DB스키마"

 

# 백업 작업
mysqldump -u$USERNAME -p$PASSWORD  $DATABASE > /backup/maria_db_backUp_${DATE}.sql 

```

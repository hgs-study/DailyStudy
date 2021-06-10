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

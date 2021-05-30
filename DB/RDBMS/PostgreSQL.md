### PostgreSQL
-----
```
- PostgreSQL(포스트-그레스-큐엘 [Post-Gres-Q-L]로 발음)은 객체-관계형 데이터베이스 시스템(ORDBMS)
- 엔터프라이즈급 DBMS의 기능과 차세대 DBMS에서나 볼 수 있을 법한 많은 기능을 제공하는 오픈소스 DBMS 
- 실제 기능적인 면에서는 Oracle과 유사한 것이 많음
- Oracle 사용자들이 가장 쉽게 적응할 수 있는 오픈소스 DBMS가 PostgreSQL이라는 세간의 평 또한 많음
```

### 설치
-----
```
# postgresql을 도커로 실행시키는 명령어
docker run --name pgsql -d -p 5432:5432 -e POSTGRES_USER=postgresql -e POSTGRES_PASSWORD=postgrespassword postgres
```

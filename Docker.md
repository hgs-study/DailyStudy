## Docker
+ Docker란?
``` 
 클라우드 환경 서버에서 VM을 여러개 두지 않아도 Docker는 각각의 Docker 애플리케이션을 '프로세스'처럼 간주한다. 
 => VM에 비해 오버헤드가 적어서, 원래의 장비 성능을 발휘할 수 있게 된다.
```

+ Docker 설치
``` 
1. 로컬 도커 윈도우즈 설치

2. 로컬 도커 실행 순서(Clone - Build - Run)대로 각각의 명령어로 실행

3. GCP에 도커 다운로드 후 도커 실행
 => sudo yum install docker
 => sudo systemctl start docker

4. 로컬에서 실행한 Getting started의 도커 명령어를 GCP SSH에 "docker run -d -p 80:80 docker/getting-started" 삽입

5. Got permission 에러 -> 1024번 포트(웰노운포트)를 열 때는 관리자 권한이 필요
 => sudo docker run -d -p 80:80 docker/getting-started 
 
6. 로컬 PC & GCP에 Docker 설치 완료
```

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
![GCP 위 도커](https://user-images.githubusercontent.com/76584547/107143326-d45fb980-6977-11eb-88e6-4ddbd30ab0fe.png)

+ Docker 이미지란?
``` 
 - 서비스 운영에 필요한 서버 프로그램, 소스코드 및 라이브러리, 컴파일 된 실행 파일을 묶은 형태
 - 특정 프로세스를 실행하기 위한 모든 파일과 설정값(환경)을 지닌 것, 더 이상의 의존성 파일을 컴파일하거나 이것저것 설치할 필요 없는 상태의 파일
 - 컨테이너 구성 시, Base Image가 깔리고 그 위에 Read Only(RO) 레이어들이 쌓이면서 최종적으로 Read Write(RW) 레이어가 쌓이는데 이제 시스템이 이를 읽고 쓸 수 있다. 이 상태를 컨테이너라고 한다. 
```

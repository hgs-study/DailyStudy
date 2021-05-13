## Docker
+ Docker란?
``` 
 클라우드 환경 서버에서 VM을 여러개 두지 않아도 Docker는 각각의 Docker 애플리케이션을 '프로세스'처럼 간주한다. 
 => VM에 비해 오버헤드가 적어서, 원래의 장비 성능을 발휘할 수 있게 된다.
```
![image](https://user-images.githubusercontent.com/76584547/108619392-e9ecdd00-7467-11eb-81fc-f4e91bcd86c7.png)

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

+ 출처 
``` 
https://tech.osci.kr/2020/03/03/91690167
https://www.notion.so/05486768ef7e41f698b6760519eb8a34
```

+ Docker 이미지 생성 & 실행
``` 
 1. 이미지 생성(프로젝트)
    => gradle : docker build --build-arg JAR_FILE=build/libs/*.jar -t hgstudy/docker_test_enum_project .
 2. 이미지 확인
    => docker images
 3. docker tag [push할 image ID or name] [docker hub ID]/[image name]:[version]
 4. docker push [docker hub ID]/[image name]

- 도커 자동 실행 : sudo systemctl enable docker

- docker run -p 8080:8080 jenkins:2.60.3 으로 포트 바꿔서 여러 컨테이너를 띄울수 잇음
- Dockerfile 명령어
 - https://www.daleseo.com/dockerfile/
- 도커 컨테이너 명령어
 - https://m.blog.naver.com/PostView.nhn?blogId=complusblog&logNo=220974632766&proxyReferer=https:%2F%2Fwww.google.com%2F
```

## Ubuntu 도커 & 젠킨스 설치
-----
### 도커
-----
![image](https://user-images.githubusercontent.com/76584547/117834960-90e85480-b2b2-11eb-9784-22141e02f4ec.png)

<br/>

※ 출처 : https://tinyurl.com/y9phv5gd (이 강의 정말 좋아요우.. - 감자튀김-)

 + 참고 : https://blog.cosmosfarm.com/archives/248/%EC%9A%B0%EB%B6%84%ED%88%AC-18-04-%EB%8F%84%EC%BB%A4-docker-%EC%84%A4%EC%B9%98-%EB%B0%A9%EB%B2%95/
 + 자바 설치 : sudo apt-get install openjdk-8-jdk

 1. sudo docker run -d -p 80:80 docker/getting-started (도커 다운로드)
  + 도커 튜토리얼 페이지
  + 80포트에 도커 getting started 페이지 띄움 (해당 서버 ip 접속해서 바로 확인 가능)
 2. sudo apt-get install openjdk-8-jdk (자바 다운로드) 
 3. sudo docker pull hgstudy/spring-boot-cpu-bound (도커 허브에 있는 spring-boot-cpu-bound 이미지 가져옴)
 4. sudo docker run -p 80:80 hgstudy/spring-boot-cpu-bound (외부 80번 포트를 내부 80번 포트로 실행)

### 젠킨스
-----
 1. sudo docker pull hgstudy/jenkins:2.60.3 (젠킨스 이미지 다운)
 2. sudo docker run --restart=always -v /etc/localtime:/etc/localtime:ro -d -p 8080:8080 jenkins/jenkins:lts (도커 시작 시 실행, 시간대 설정)
 3. sudo docker exec -it 5a236a157b69 /bin/bash (해당 젠킨스 컨테이너 접속)
 4. cat /var/jenkins_home/secrets/initialAdminPassword (젠킨스 비밀번호)
 5. jenkins 접속
 6. 플러그인 (Publish Over SSH 설치)
 7. 새로운 item (프리스타일 프로젝트 생성)
 8. Github Integeration Plugin 설치
 9. Jenkins 설정 (빌드 유발 탭 이동 >> GitHub hook trigger for GITScm polling 체크)



※ 참고 (도커&젠킨스&깃허브) : https://velog.io/@jangky000/Docker-Jenkins-%EC%9E%90%EB%8F%99%EB%B0%B0%ED%8F%AC-%EC%A0%95%EB%A6%AC

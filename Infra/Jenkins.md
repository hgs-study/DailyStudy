## Jenkins
+ Jenkins란?
``` 
CI & CD , Batch
 ```
 
 + Jenkins 설치 (GCP 이용)
``` 
  1. sudo yum install wget (wget 설치)
  2. sudo yum install maven (maven 설치)
  3. sudo yum install git (git 설치)
  4. sudo yum install docker (docker 설치)
  5. 
  sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo (젠킨스를 위한 패키지 설치)
  sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key 
  sudo yum install jenkins (젠킨스 설치)
  sudo systemctl start jenkins (젠킨스 데몬 실행 명령어)
  sudo systemctl status jenkins (젠킨스 데몬 상태 확인)
  
  
ㅇ jenkins
- 계정명 : 
- 암호 : 
- 이름 : 
- 이메일 주소: 
키 생성 : ssh-keygen -t rsa -f ~/.ssh/id_rsa
개인키 : /home/hgstudyy/.ssh/id_rsa.
공개키 : /home/hgstudyy/.ssh/id_rsa.pub.

ㅇ worker 인스턴스
- 사용자 : hgstudyy
worker ssh키 설정
=> vi ~/.ssh/authorized_keys 여기에 젠킨스 공개키 넣음

=> chmod 700 ~/.ssh
=> chmod 600 ~/.ssh/authorized_keys

ㅇ 젠킨스 설정
=> 젠킨스 Publish over SSH에 젠킨스 개인키 설정 

ㅇ worker 인스턴스
 => sudo yum install 도커 설치
 => sudo systemctl start docker
 => sudo chmod 666 /var/run/docker.sock

nohup docker run -p 8080:80 hgstudy/spring-boot-cpu-bound > /dev/null 2>&1 &
nohup~ &
- 백그라운드 실행시킨다
 ```

### 젠킨스 세팅
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

※젠킨스 세팅 : https://velog.io/@ifthenelse/jenkins-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-ubuntu-20.04

※ 참고 (도커&젠킨스&깃허브) : https://velog.io/@jangky000/Docker-Jenkins-%EC%9E%90%EB%8F%99%EB%B0%B0%ED%8F%AC-%EC%A0%95%EB%A6%AC
※ github & 젠킨스 연동 : https://goddaehee.tistory.com/258
※ github(private) & 젠킨스 연동 : https://www.skyer9.pe.kr/wordpress/?p=522

### 한 인스턴스 안에 도커 + 젠킨스
-----
 1. java 설치
 2. 젠킨스 설치
 3. 도커 설치 


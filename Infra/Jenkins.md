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

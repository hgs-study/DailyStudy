## Jenkins
+ Jenkins란?
``` 
 클라우드 환경 서버에서 VM을 여러개 두지 않아도 Docker는 각각의 Docker 애플리케이션을 '프로세스'처럼 간주한다. 
 => VM에 비해 오버헤드가 적어서, 원래의 장비 성능을 발휘할 수 있게 된다.
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
  
 ```

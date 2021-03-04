## AWS
+ AWS 설치 순서
``` 
  1. EC2 인스턴스 시작 (Amazon Linux 2)
    - t2.micro
    - 스토리지 크기 (8gb -> 30gb 변경)
    - Name 태그 추가 (인스턴스 이름 변경)
    - 보안 그룹 생성 (SSH : 22 내IP , HTTP: 80 0.0.0.0/0(기본) ,HTTPS 443 0.0.0.0/0(기본)
    - pem 키 저장
    
  2. EIP 할당 (Elastic IP, 탄력적 IP)
    - 사용할 인스턴스가 없을 때도 비용이 청구되기 때문에 잊지 말고 꼭 삭제해야한다.
    
  3. EC2 접속
    - Window : puttygen에서 pem키를 ppk파일로 변환
    - ec2-user@탄력적ip (port:22) 접속
    
  4. EC2 환경설정
    ㅇ java 설치
      - sudo yum install -y java-1.8.0-openjdk-devel.x86_64
    ㅇ 타임존 변경 (세계 표준 시간 -> 한국시간)
      - sudo rm /etc/localtime
      - sudo ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime
    ㅇ HostName 변경
      - sudo hostnamectl set-hostname (본인이 원하는 서비스명 or EC2 이름)
      - HOSTNAME=(본인이 원하는 서비스명 or EC2 이름) 추가
      - 재부팅(sudo reboot)
      
  5. Git 설치
    - sudo yum install git
    - git --version
    - mkdir ~/app && mkdir ~/app/step1
    - cd ~/app/step1
    - git clone 깃 레파지토리 주소
    - cd 프로젝트명
    - ll(클론 확인)
    - ./gradlew test (Permission denied 에러시 "chmod +x ./gradlew" 명령어를 먼저 삽입)
  
  6. 배포 스크립트 생성
    - vim ~/app/step1/deploy.sh
    - chmod +x ./deploy.sh
```

+ deploy.sh
```
#!/bin/bash

REPOSITORY=/home/ec2-user/app/step1
PROJECT_NAME=SpringSecurity-Basic

cd $REPOSITORY/$PROJECT_NAME/

echo "> Git Pull"

git pull

echo "> 프로젝트 build 시작"

./gradlew build

echo "> step1 디렉토리로 이동"

cd $REPOSITORY

echo "> Build 파일 복사"

cp $REPOSITORY/$PROJECT_NAME/build/libs/*.jar $REPOSITORY/

echo "> 현재 구동중인 애플리케이션 pid 확인"

CURRENT_PID=$(pgrep -fl SpringSecurity-Basic | grep jar | awk '{print $1}')

echo "현재 구동중인 어플리케이션 pid: $CURRENT_PID"

if [ -z "$CURRENT_PID" ]; then
    echo "> 현재 구동중인 애플리케이션이 없으므로 종료하지 않습니다."
else
    echo "> kill -15 $CURRENT_PID"
    kill -15 $CURRENT_PID
    sleep 5
fi

echo "> 새 어플리케이션 배포"

JAR_NAME=$(ls -tr $REPOSITORY/*.jar | tail -n 1)

echo "> JAR Name: $JAR_NAME"

echo "> $JAR_NAME 에 실행권한 추가"

chmod +x $JAR_NAME

echo "> $JAR_NAME 실행"

nohup java -jar $JAR_NAME 2>&1 &
```

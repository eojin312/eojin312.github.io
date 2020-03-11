---
title: "ec2에서 배포하기 2탄"
date: 2020-03-09 16:20:28 -0400
categories: 개인공부
---

책에서는 이렇게 나온다
````console
vim ~/app/git/deploy.sh
````
maven 버전용으로 배포 파일 작성했다

```console
#!/bin/bash


cd $REPOSITORY/freelec-springboot-webservice/

echo ">Git Pull"

git pull

echo ">프로젝트 install 시작"

./mvn package

echo ">Build 파일 복사"

cp ./target/*.jar $REPOSITORY/

echo ">현재 구동중인 애플리케이션 pid 확인"
CURRENT_PID=$(pgrep -f freelec-springboot-webservice)
echo "$CURRENT_PID"
if[ -z $CURRENT_PID ]; then
        echo ">현재 구동중인 애플리케이션이 없으므로 종료하지 않습니다"
else
        echo ">kill -15 $CURRENT_PID"
        kill -15 $CURRENT_PID
        sleep 5
fi

echo "> 새 애플리케이션 배포"

JAR_NAME = $(ls -tr $REPOSITORY/ | grep *.jar | tail -n 1)

nohup java -jar $REPOSITORY/$JAR_NAME 2>&1 &
```




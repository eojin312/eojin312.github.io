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

echo "> JAR Name : $JAR_NAME"

nohup java -jar $REPOSITORY/$JAR_NAME 2>&1 &
```

저장 후 나가려 했는데 아니 뭔;

![스크린샷 2020-03-09 오후 4 07 21](https://user-images.githubusercontent.com/45488643/76190422-4be51000-6220-11ea-841d-ec1ef0f06374.png)

봤더니 책대로 하니 문제였던 것이다 git 파일을 만들지도 않았는데 git의 step1 의 deploy.sh 를 실행시켜서 저장하려고 하니 저장이 안되는게 당연하다

그래서 정석대로 git 파일을 따로 만들고 다시 배포했더니 잘 돌아간다


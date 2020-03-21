---
title: "백그라운드에서 스프링 부트 띄우기"
date: 2020-03-22 13:20:28 -0400
categories: 개인공부
---
## 배경
갑자기 서비스 접속이 안된다는 제보를 받았습니다

처음에 ec2 로 들어가서 띄어져있는 상태인지 확인해봤습니다
```console
ps -ef | grep java
```
두둥..! 아무것도 안떠져있는 것을 확인..!

찾아보니 & 로 띄울 경우 터미널 로그아웃 시 같이 종료된다고....
그래서 nohup으로 띄워야 한다는 것을 알게되었습니다

```console
nohup java -jar /home/ec2-user/.m2/repository/hachi/parking-app/0.0.1-SNAPSHOT/parking-app-0.1-SNAPSHOT.jar > log.out &
```

그리고 나서 터미널을 exit로 빠져나오고나서 브라우저로 테스트 -> 정상!!!

다른 터미널에서 tail -f log.out 으로 스프링부트 로그도 실시간으로 확인할 수 있었습니다!!
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

+ nohup 으로 하고 enter 두 번 치니 or ctrl + C 를 하니 [1]Exit 1 nohup java -jar *.jar >> ~/log.out 2>&1 이렇게 떴습니다.
+ tail -f log.out 에서 로그 확인 중 에러가 발생합니다

```console
***************************
APPLICATION FAILED TO START
***************************

Description:

Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.

Reason: Failed to determine a suitable driver class
```

저같은 경우 thymeleaf 설정을 application.property 에 하고 db 설정은 application.yml 에서
했는데 springboot 에선 한 곳에서만 설정해야한다는 것을 미처 몰랐습니다.

application.yml 에 모든 설정을 세팅하니 해결 완료!

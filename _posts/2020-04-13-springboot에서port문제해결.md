---
title: "[MAC] SpringBoot 에서 Web server failed to start. Port 8080 was already in use. 문제 해결"
date: 2020-04-13 13:20:28 -0400
categories: 회고
---

SpringBoot 에서 웹 서버로 실행할 때 가끔 Web server failed to start. Port 8080 was already in use.
에러가 생깁니다. 이건 에러 메시지 그대로 **8080 포트가 실행 중일 때 SpringApplication 을 Run 하면 발생하는 에러입니다**

생각보다 해결법은 간단합니다 
터미널을 열고 실행되고있는 Port 를 죽이면 됩니다!

일단 8080 으로 띄어져있는지 확인해봤습니다.
```console
$ netstat -na | grep 8080

tcp46      0      0  *.8080     LISTEN     
```
LISTEN 이라고 뜨면 띄어져있는 것입니다.

다음은 java 만 찾아서 PID 를 죽여보겠습니다
```console
$ ps -ef| grep java

501 10531 1  0  ~~생략~~ hachishop.HachishopApplication
501 10629 10613 ~~생략~~ maven.server.RemoteMavenServer
```

제가 지금 죽여야할 놈은 SpringApplication 이기에 HachishopApplication 의 PID 찾아서 죽여야합니다.

```console
$ kill -15 10531
```


## 명령어의 뜻
ps 는 mac 의 활성상태보기와 같습니다. 거기서 java 만 grap 해와! 라는 뜻이죠
netstat 는 네트워크 연결상태, 인터페이스 상태 등을 보여주는 명령어인데 실행중인 8080 포트가 있는 지 확인해봐! 라는 뜻입니다.
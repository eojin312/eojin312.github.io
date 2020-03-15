---
title: "ec2 maven 컴파일 오류"
date: 2020-03-06 12:20:28 -0400
categories: 회고
---

EC2 에 maven 을 설치(EC2에 메이븐 설치하기 링크--준비중)를 했다
책의 가이드대로 mvn test 를 돌려봤는데, compile에러가 발생..두둥....ㅠㅠ

```console
[INFO] --- maven-compiler-plugin:3.5.1:compile (default-compile) 
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 6.805 s
[INFO] Finished at: 2020-03-05T13:23:54Z
[INFO] Final Memory: 23M/55M
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.5.1:compile (default-compile) on project: Fatal error compiling: invalid target release: 1.8 -> [Help 1]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging. 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException
``` 

당황했다.. 
혹시나 maven-complier-plugin 이 없는 건지 아니면 깔려져있는데 문제가 생긴 것인지 pom.xml을 확인하고
구글링한대로 build의 plugin부분을 이러저리 바꿔보았지만 에러메시지는 요지부동이었다...와나...

그러다가 유력한 원인인 것 같은 걸 stackoverflow 에서 발견!!
자바 버전이 다를 수 있다는 것이다..

일단
나는 EC2에 자바8을 따로 설치했었다.
pom에 설정된 자바버전도 자바8이다.


그러면, EC2의 자바 버전이 달라서 그런가?? 나는 자바8을 설치했는데??? 일단 확인부터 고고


컴파일 단계 오류였으니... javac가 어디에 있는지 확인부터
 ```console
$ which javac
/usr/bin/javac
```

readlink로 위치 확인해보자..어디있느냐...
```console
$ readlink -f /usr/bin/javac
/usr/lib/jvm/java-1.7.0-openjdk-1.7.0.231.x86_64/bin/javac
```

어랏!!! 1.7이네... 근데.. java는 버전이 머지?
```console
$ which java
/usr/bin/java
$ readlink -f /usr/bin/java
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.50.amzn1.x86_64/jre/bin/java
```

아... java8을 설치해서 java는 8을 가리키고 있는데,
javac는 7이었고, mvn test 시 compile이 선행되어야하는데 7로 컴파일하니까 에러가 나지...쩝..


그럼 javac를 7에서 8로 어떻게 바꾸지??

일단 JAVA_HOME이 어딘지부터 확인해보자
```console
$ echo $JAVA_HOME
/usr/lib/jvm/java
```

/usr/lib/jvm/java 요기에 있는 /bin에 있는 java -version을 해보니 이 녀석이 7이다..
```console
$ pwd
/usr/lib/jvm/java/bin
$ ./java -version
java version "1.7.0_231"
OpenJDK Runtime Environment (amzn-2.6.19.1.80.amzn1-x86_64 u231-b01)
OpenJDK 64-Bit Server VM (build 24.231-b01, mixed mode)
```
일단, 메이븐이 컴파일할때 JAVA_HOME을 참조해서 bin밑에 있는 javac를 찾아갈 것이다.
그래서 JAVA_HOME을 새로설치했던 8로 바꿔주면 해결될 듯 싶다.

그래서 /etc/profile에 JAVA_HOME과 PATH, CLASS_PATH를 모두 8 경로로 바꿔준다.
/etc/profile의 맨 끝에 저 3줄을 붙여줬다
```console
$ sudo vi /etc/profile
~~~~ 중략 ~~~~~
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.50.amzn1.x86_64/jre
export PATH=$JAVA_HOME/bin/:$PATH
export CLASS_PATH=$JAVA_HOME/lib/:$CLASS_PATH
```

그 다음에 source라는 명령어로 적용을 시키고, ec2를 재시작!! 고고~~~
```console
$ source /etc/profile
$ reboot now
```
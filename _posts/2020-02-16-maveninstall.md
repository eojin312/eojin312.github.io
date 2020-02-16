---
title: "maven install"
date: 2020-02-16 12:22:28 -0400
categories: 개인공부
---

어제와 달리 오늘은 maven 을 mac 에 깔려한다.
터미널에서 brew install maven 을 하면 알아서 찾아 깔아준다
다 깔았다고 하면 꼭 버전을 확인해주자

````console
user$ mvn -version

Apache Maven 3.6.3 
````

그 다음은 메이븐 설치 위치를 확인한다

```
/usr/local/Cellar/maven/3.6.3_1/bin
```

다음은 환경변수를 등록해준다

```
 vi .bash_profile 
```

그리고 단축키 shift + G 를 눌러 커서를 맨 아래로 향하게 한 다음 알파벳 O 를 눌러
insert 할 수 있게 바꿔준다 그 다음 차례대로 환경변수를 설정하면 된다.

```
export MVN_HOME=/usr/local/Cellar/maven/3.6.3_1
export MVN=$MVN_HOME/BIN
export PATH=$PATH:$MVN_HOME/bin
``` 

그 후에는 터미널을 껐다 다시 실행하거나 . .bash_profile 을 입력해서 강제로 실행시켜줘야한다
그렇게 하지않으면 환경변수를 지정할 수 없다.


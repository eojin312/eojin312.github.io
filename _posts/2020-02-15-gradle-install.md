---
title: "gradle 적용 삽질기"
date: 2020-02-15 13:49:28 -0400
categories: 개인공부
---

"스프링 부트와 AWS로 혼자 구현하는 웹서비스" 책으로 공부를 시작
예제가 gradle이다!!

intellij내장 gradle은 4.8인데.. 첫 빌드부터 에러가 남.. 4.8 이상 버전을 설치해야한다고하네..

그래서

https://gradle.org/install/ 방문

시키는대로 설치 시작...

step.01
sdk install gradle 6.1.1 --> 에러 sdk 가 없다고 함 뭔지 잘 모르겠지만  Software Development Kits on most Unix-based systems. 요 문구로 봐서는 맥은 대상이 아닌가봄

Homebrew is “the missing package manager for macOS”. -> 이건 맥에서 되겠네..라고 생각
brew install gradle

막 homebrew도 업데이트하고, 로그에 보니까 openjdk13도 지가 막 다운로드하고 
내가 뭘해야할지 가이드도 로그에 나옴

For the system Java wrappers to find this JDK, symlink it with
  sudo ln -sfn /usr/local/opt/openjdk/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk.jdk

openjdk is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

If you need to have openjdk first in your PATH run:
  echo 'export PATH="/usr/local/opt/openjdk/bin:$PATH"' >> ~/.bash_profile


  export PATH="/usr/local/opt/openjdk/bin:$PATH"

For compilers to find openjdk you may need to set:
  export CPPFLAGS="-I/usr/local/opt/openjdk/include"

그래서 시키는대로 함
1. 심볼릭 잡기
sudo ln -sfn /usr/local/opt/openjdk/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk.jdk

2. PATH에 2개 추가
> cd ~
> vi .bash_profile

```console
export PATH="/usr/local/opt/openjdk/bin:$PATH"
export PATH="/usr/local/opt/openjdk/bin:$PATH"
```

step3에 나온 gradle bin디렉토리를 환경변수 PATH 에 추가 (.bash_profile)
```console
export PATH="/usr/local/opt/openjdk/bin:$PATH"
export CPPFLAGS="-I/usr/local/opt/openjdk/include"
export GRADLE_USER_HOME="/usr/local/Cellar/gradle/6.1.1_1"
export PATH=$PATH:/opt/gradle/gradle-6.1.1/bin
```

그 담에 intllij로 돌아와서
intellij에서 jdk를 openjdk13으로 모두 변경
gradle 설정을
Use default gradle wrapper (recommended)로 설정

이제 다 되었다고 생각했는데 안됨!!

그래서 겁나 구글링

그래서 찾은 방법은

프로젝트 홈디렉토리에 gradlew 라는 실행파일 있음

```console
userui-MacBookPro:fr-springboot-start user$ pwd
/Users/user/IdeaProjects/fr-springboot-start
userui-MacBookPro:fr-springboot-start user$ ls -la
total 40
drwxr-xr-x  10 user  staff   320B Feb 15 13:23 .
drwxr-xr-x  39 user  staff   1.2K Feb 15 11:56 ..
drwxr-xr-x   8 user  staff   256B Feb 15 13:19 .gradle
drwxr-xr-x   5 user  staff   160B Feb 15 13:29 .idea
-rw-r--r--   1 user  staff   968B Feb 15 13:23 build.gradle
drwxr-xr-x   3 user  staff    96B Feb 15 11:31 gradle
-rwxr-xr-x   1 user  staff   5.6K Feb 15 13:20 gradlew
-rw-r--r--   1 user  staff   2.9K Feb 15 13:20 gradlew.bat
-rw-r--r--   1 user  staff    42B Feb 15 11:30 settings.gradle
drwxr-xr-x   4 user  staff   128B Feb 15 11:31 src
```
아래첨하면 프로젝트의 gradle버전이 변경?? 되는 것 같은데, 계속 4.8로 나옴
./gradlew wrapper --gradle-version 6.1.1

본래 gradlew가 잘 실행되면 아래 파일에 distributionUrl 가 6.1.1로 자동으로 변경된다고하는데 내껀 계속 4.8 ㅠㅠ
gradle/wrapper/gradle-wrapper.properties

그래서
distributionUrl=https\://services.gradle.org/distributions/gradle-4.8-bin.zip
을
distributionUrl=https\://services.gradle.org/distributions/gradle-6.1.1-bin.zip
로 강제로 바꿈

근데 또 안댐!!! 머냐!!!
```console
userui-MacBookPro:fr-springboot-start user$ ./gradlew wrapper --gradle-version 6.1.1
Exception in thread "main" java.lang.RuntimeException: Base: GRADLE_USER_HOME distributionPath=wrapper/dists is unknown
        at org.gradle.wrapper.PathAssembler.getBaseDir(PathAssembler.java:97)
        at org.gradle.wrapper.PathAssembler.getDistribution(PathAssembler.java:43)
        at org.gradle.wrapper.Install.createDist(Install.java:44)
        at org.gradle.wrapper.WrapperExecutor.execute(WrapperExecutor.java:107)
        at org.gradle.wrapper.GradleWrapperMain.main(GradleWrapperMain.java:61)
```

GRADLE_USER_HOME은 또 머냐???
일단 임시로 환경변수 추가

```
user$ export GRADLE_USER_HOME="/usr/local/Cellar/gradle/6.1.1_1"
```

그 다음 혹시 gradle/wrapper/gradle-wrapper.properties을 열어놔서 문제일까봐 창을 닫고

다시 ./gradlew wrapper --gradle-version 6.1.1 실행
오!!
된다!! 빌드가!!!

```console
userui-MacBookPro:fr-springboot-start user$ ./gradlew wrapper --gradle-version 6.1.1
Downloading https://services.gradle.org/distributions/gradle-6.1.1-bin.zip
............................................................................................

Welcome to Gradle 6.1.1!

Here are the highlights of this release:
 - Reusable dependency cache
 - Configurable compilation order between Groovy/Kotlin/Java/Scala
 - New sample projects in Gradle's documentation

For more details see https://docs.gradle.org/6.1.1/release-notes.html

Starting a Gradle Daemon (subsequent builds will be faster)

Deprecated Gradle features were used in this build, making it incompatible with Gradle 7.0.
Use '--warning-mode all' to show the individual deprecation warnings.
See https://docs.gradle.org/6.1.1/userguide/command_line_interface.html#sec:command_line_warnings

BUILD SUCCESSFUL in 52s
1 actionable task: 1 executed

```

엄청 빡시다.. 메이븐보다 누가 더 쉽고 간단하다고 했는가..ㅠㅠ
뭔가 빠른 버전업에 따라 몇개월 전 구글링 자료도 못믿을 수 있는 것 같다
쨋든 최신 버전을 사용해야하니 구글링할때 기간을 1개월 이내로 설정해두고 검색해야겠다.
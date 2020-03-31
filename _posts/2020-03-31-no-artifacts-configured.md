---
title: "No Artifacts Configured"
date: 2020-03-31 13:20:28 -0400
categories: 회고
---
 
프로젝팅 세팅 중 tomcat 에러가 생깁니다

헉 어떻게 해야하지 fix 를 눌러도 아무것도 안뜨는데ㅠㅠ

하지만 걱정할 필요 없습니다! 이건 간단하고 쉽게 해결할 수 있습니다

![스크린샷 2020-03-31 오후 2 11 29](https://user-images.githubusercontent.com/45488643/77989482-e6d19580-7359-11ea-97b1-1538edf6ad61.png)

maven 이라면 pom.xml 에 들어가서 package 를 war 로 바꿔줘야합니다. 서버에 배포할 파일형식을 war 로 지정해주는 것이죠

```xml
<project>
    <!--중간생략-->
    <packaging>war</packaging>
</project>
```

다시 Edit Configuration 에 들어갑니다.

![스크린샷 2020-03-31 오후 2 18 04](https://user-images.githubusercontent.com/45488643/77989735-78410780-735a-11ea-9092-c9a2076ce642.png)

그러고 다시 fix 를 누르면 두가지 war 가 뜰 것입니다.

![스크린샷 2020-03-31 오후 2 19 40](https://user-images.githubusercontent.com/45488643/77989814-ae7e8700-735a-11ea-9fc6-15c6c1b9d7b4.png)


여기서 exploded 버전으로 눌러주시면 끝!


---
title: "내가 공부한 springboot 로 server port 바꾸기"     
date: 2020-08-21 13:20:28 -0400
categories: 공부
---

> spring 에서는 server.xml 에서 포트를 변경해주면 되지만 springboot는 server.xml 이 없습니다.
ㅋㅋㅋ 이거 뭐.. 어떻게 포트를 바꾸라는 건지 잘 모르겠는데 springboot 의 설정파일은 어디서 변경할 수 있나..궁금해지기 시작합니다.
어디서 변경할 수 있을까요?

# 생각보다 쉬워요.

application.properties 혹은 application.yml 을 이용하면 됩니다.
정답을 알려주기 전에 application.properties, //.yml 에 대해서 알려드리고자합니다.
 
## application.properties 는 뭔가요?

application 은 외부설정파일입니다. application에서 사용하는 여러가지 설정 값들을 application 내 외에서 정의할 수 있습니다.

# 본론

다시 본론으로 넘어옵시다. 이제 서버 변경 하는 방법을 알려드리겠습니다. properties 용, yml 용 따로 알려드리겠습니다.

**application.properties**

```properties
server.port = 포트번호
```

**application.yml**
```yaml
server:
  port: 8080
```

생각했던 것보다 훨씬 쉬웠습니다~~!!

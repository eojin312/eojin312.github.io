---
title: "Unable to start embedded Tomcat"     
date: 2020-06-16 13:20:28 -0400
categories: 공부
---
jpa 와 spirngboot 로 프로젝트 작업 중 갑자기 **Unable to start embedded Tomcat**
이런 에러가 뜨면서 springApplication 이 실행되지않았습니다. springboot에 내장된 tomcat 이 단 한번도 말썽을 일으킨 적이 없어서
더욱 더 놀랜 기억이 있습니다.

방법을 찾아보니 대부분 pom.xml 에 tomcat 을 걸어줍니다
```xml
<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
</dependency>
```
이런 방법도 있지만 jpa 를 사용하시면 이 방법으로 해결안되고 오히려 프로젝트가 더욱 꼬입니다.

사실 원인은 간단했습니다. jpa로 정규화 연습을 하려고
```java
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "user_no")
    private Long id;
    @Column(nullable = false)
    private String name;
    @Column(nullable = false)
    private String email;

    @OneToMany
    @JoinColumn
    private User user;
}
```

이런 식으로 연습했더니 springApplication 이 실행되지않고 에러가 났습니다.
jpa 로 정규화는 신중히 해야 이런 당황스런 에러를 겪지않습니다.

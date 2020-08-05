---
title: "Argument ReturnType"     
date: 2020-07-30 13:20:28 -0400
categories: 공부
---
# Handler Method Argument
주로 요청 그 자체 또는 요청에 들어있는 정보를 받아드리는 곳에 사용한다

## Stream
단일 방향으로 연속적으로 흘러가는 것

## InputStream
서블릿 api 는 사용하고싶지않지만 요청 본문만 사용하고싶을 때.

## OutPutStream
InputStream 이 데이터를 읽어오면 => Reader
OutPutStream 어떤 데이터를 쓰면 응답 본문에 쓰게 됨 => Writer

## Spring 5 에 추가 된게 PushBuilder
Http2 에서 사용가능

한번 요청하면 events 요청 들어왔을 때 보여주는 view 에서 사용하는 리소스를
브라우저가 리소스 url 을 서버에 다시 요청하는데 하지만 pushbuilder 를 사용하면
추가 데이터를 서버가 능동적으로 push 를 미리 할 수 있다
그러면 브라우저가 서버에 요청을 안해도 된다.

```java
@GetMapping("/post")
public String list(PushBuilder pushBuilder) {
    pushBuilder.path("/js/jquery-3.5.1.min.js").push();
    return "post/list";
}
```



# 결론

- pushbuilder 이 리소스를 필요로 할거야 예상하고 미리 보내주는 기능이다.


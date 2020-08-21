---
title: "request param에 대해서"
date: 2020-02-24 12:00:28 -0400
categories: 개인공부
---

## 뜻
@requestparam 은 어노테이션은 HttpServletRequest 객체와 같은 역할을 합니다.
servlet 에서는 HttpServletRequest 객체의 getParameter() 메소드를 사용해서 받아왔지만
스프링은 @requestParam 을 이용해서 받아옵니다.

## 예제

```java
    @GetMapping("/hello/dto")
    public HelloResponseDto helloDto(@RequestParam(value = "name") String name, @RequestParam(value = "amount") int amount) {
        return new HelloResponseDto(name, amount);
    }
```    
@RequestParam("가져올 데이터의 이름") (데이터타입) [가져온데이터를 담을 변수]

주소창에서 파라미터를 넘기지않으면 **400 에러**가 뜹니다
```
localhost8080/hello/dto?name=hachi&amount=1
```
이를 방지하기 위해서 필수적이지않도록 DefaultValue 를 지정할 수 있습니다
```java
    @GetMapping("/hello/dto")
    public HelloResponseDto helloDto(@RequestParam(value = "name", defaultValue = "hachi") String name, @RequestParam(value = "amount", defaultValue = "100") int amount) {
        return new HelloResponseDto(name, amount);
    }
```
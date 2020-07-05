---
title: "IllegalStateException: Ambiguous mapping. Cannot map 'Controller'method"     
date: 2020-07-05 13:20:28 -0400
categories: 공부
---

REST API 를 적용하고 테스트를 돌려봤는데 **IllegalStateException: Ambiguous mapping. Cannot map 'Controller'method** 라며
에러가 발생했습니다.

**PostController**
```java
@Controller
public class PostController {
    @GetMapping("/post/{id}")
    public String detail(@PathVariable Long id, Model model, @AuthenticationPrincipal AuthUser authUser) {
        model.addAttribute("id", id);
        model.addAttribute("authUser", authUser);
        return "post/detail";
    }

    @GetMapping("/post/{id}")
    public String update(@PathVariable Long id, Model model) {
        model.addAttribute("id", id);
        return "post/update";
    }
}
```
에러를 발생시킨 코드는 이렇습니다.

그 후 테스트 or 실행을 하면.. 이런 식의 에러가 발생합니다.
![스크린샷 2020-07-05 오전 11 33 58](https://user-images.githubusercontent.com/45488643/86524296-c6aef680-beb3-11ea-8993-9321ebbf8e3a.png)

해석해보니 detail 메소드와 update 메소드의 맵핑 url 이 겹치니 Spring이 헷갈린다고 application 도 실행시키지 않은 것입니다..!

## 해결방법

사실 해결방법은 간단합니다. url을 겹치지않게만 하면 되는 것입니다! 에러난 메시지를 안읽어보면 controller bean 이 생성되지 않았다고 오해할까봐 적어둡니다.


**해결한 PostController**
```java
@Controller
public class PostController {
    @GetMapping("/post/{id}")
    public String detail(@PathVariable Long id, Model model, @AuthenticationPrincipal AuthUser authUser) {
        model.addAttribute("id", id);
        model.addAttribute("authUser", authUser);
        return "post/detail";
    }

    @GetMapping("/post/update/{id}")
    public String update(@PathVariable Long id, Model model) {
        model.addAttribute("id", id);
        return "post/update";
    }
}
```

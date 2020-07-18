---
title: "http 메소드 요청 맵핑하기"     
date: 2020-07-18 13:20:28 -0400
categories: 공부
---

http 메소드는 클라이언트가 웹 서버에게 사용자 요청의 목적/종류를 알리는 수단 입니다

대표적으로 GET, POST, PUT, DELETE 정도 있습니다.

## GET
URI 형식으로 웹 서버측 리소스(데이터)를 요청합니다.

그렇기에 주로 홈페이지를 띄울 때 사용됩니다.
```java
@Controller
public class PostController {

    @GetMapping("/post")
    public String list() {
        return "post/list";
    }
}
```

## POST
파라미터를 넘겨서 해당하는 본문형식을 받습니다.

그렇기에 주로 홈페이지에서 서버로 파라미터를 넘길 상황인 회원 회원가입이나 글 작성할 때 주로 사용합니다


## GET 과 POST 의 차이점

GET 메소드도 파라미터를 넘길 수 있습니다. 다만, POST 와는 다른 방식으로 넘깁니다.

GET 은 입력한 데이터 값을 URL에 붙여서 전송합니다.

![스크린샷 2020-07-18 오후 6 26 17](https://user-images.githubusercontent.com/45488643/87849522-41e8c180-c924-11ea-8ee1-0116bdc4a04e.png)

GET 방식과 달리 POST 방식은 입력한 데이터를 본문에 포함해서 전송하기 때문입니다.

![스크린샷 2020-07-18 오후 6 28 05](https://user-images.githubusercontent.com/45488643/87849546-6e9cd900-c924-11ea-9437-efb0e8d390e3.png)

출처 : [세상의 모든 기록](https://all-record.tistory.com/100)

백기선님 강좌를 보여 짠 간단한 예제 코드입니다
```java
public class WhiteShipSampleController {

    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }
}
```

http 메소드를 테스트하기
```java
@WebMvcTest
class WhiteShipSampleControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void 요청테스트() throws Exception {
        mockMvc.perform(get("/hello"))
                .andDo(print())
                .andExpect(status().isOk())
                .andExpect(content().string("hello"));

        mockMvc.perform(put("/hello"))
                .andDo(print())
                .andExpect(status().isMethodNotAllowed())
                .andExpect(content().string("hello"));

        mockMvc.perform(post("/hello"))
                .andDo(print())
                .andExpect(status().isMethodNotAllowed())
                .andExpect(content().string("hello"));
    }
}
```
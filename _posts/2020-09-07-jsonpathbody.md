---
title: "json body 확인하기"     
date: 2020-09-16 13:20:28 -0400
categories: 공부
---

> controller test 를 할 때 브라우저에서 넘어온 값을 확인할 때가 있습니다. 가끔! 테스트를 보면 실패할 때가 빈번히 발생합니다.
그럼 우리는 주로 브라우저에서 넘어온 값이 잘 넘어왔는지 확인합니다. 그래서 오늘 얘기해볼 건 json body 를 console 창에서 번거롭게 확인하지말고
편하게 사이트에서 json 데이터를 확인해보는 걸 할 것 입니다!


먼저 테스트부터 있어야겠죠? 제가 짠 테스트 코드를 예시로 들겠습니다.
전체 글 리스트를 담아내는 기능 테스트입니다,
```java
@Test
void 리스트가_200OK() throws Exception {
    mockMvc.perform(get("/api/posts"))
            .andExpect(print())
            .andExpect(status().isOk());
}
```
테스트 코드에 보면 print() 라고 있는데 이 메소드는 테스트 결과 창에 json body 데이터를 출력해줍니다

![스크린샷 2020-09-16 오후 6 28 56](https://user-images.githubusercontent.com/45488643/93319201-8de3b400-f84a-11ea-80b1-9dbebdb89cb5.png)
이렇게요!! 만약에 테스트가 오류가 났다면.. 뭐가 문제인지 알아보기위해 이 줄을 전체 복사해줍시다.


여기서 복사했던 정보를 붙여넣어주면 알아서 보기 좋은 json 형태로 변환해줍니다.
![스크린샷 2020-09-06 오후 3 22 17](https://user-images.githubusercontent.com/45488643/93318448-af906b80-f849-11ea-8d3b-ed9eb9fa7d2f.png)

[https://jsonformatter.curiousconcept.com/#](https://jsonformatter.curiousconcept.com/#)

위에 사이트에서 복사한 보기 좋은 json 데이터를 이 사이트에 붙여넣고 확인해보고싶은 데이터들을 명령어를 쳐서 확인하시면 됩니다!
![스크린샷 2020-09-06 오후 3 23 10](https://user-images.githubusercontent.com/45488643/93317393-60960680-f848-11ea-906a-f860c169fdf6.png)

[http://jsonpath.herokuapp.com/](http://jsonpath.herokuapp.com/)



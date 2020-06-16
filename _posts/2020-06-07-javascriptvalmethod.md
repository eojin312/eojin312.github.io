---
title: "javascript val()"     
date: 2020-06-07 13:20:28 -0400
categories: 공부
---

## javascript val()

val()은 form의 값을 가져오거나 값을 설정하는 메소드입니다.

제가 만드는 프로젝트에서 순수 jquery 와 javascript 를 사용해서
게시판을 만들 때 val() 메소드를 정말 자주 사용합니다.

```javascript
this.title = $('#title').val();
```
id 값이 title 인 값을 가져와서 변수 title 에 담아줍니다.

**파라미터가 있으면 set 없으면 get 이라고 이해했습니다.**

    값을 변경 할 때 : $(변경할 부분 선택).val(값);
    값을 가져 올 때 : $(변경할 부분 선택).val();




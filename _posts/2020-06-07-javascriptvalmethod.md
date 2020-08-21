---
title: "jquery val()"     
date: 2020-06-07 13:20:28 -0400
categories: 공부
---
>제가 만드는 프로젝트에서 순수 jquery 를 사용해서
게시판을 만들 때 val() 메소드를 정말 자주 사용합니다.
그래서 val() 은 어떤 동작을 하는지 설명해보겠습니다!

## 의미

val()은 form의 값을 가져오거나 값을 설정하는 메소드입니다.

## 예제
코드는 이렇습니다.
```html
<html>
    <head>
    
    </head>
    <body>
        <div class="form-group">
            <label>제목</label>
            <input class="form-control" id="title" type="text">
        </div>
    </body>
</html>
```
```javascript
$('#title').val(post.title);
```
html 태그 속성 중 id ='title' 로 된 곳에 값이 전달되면 그 값으로 js 로 에 넘겨지고,
js 에서 지지고볶은 후 그 결과를 html 에 최종 반환됩니다. 

**파라미터가 있으면 set 없으면 get 이라고 이해했습니다.**
 
    값을 변경 할 때 : $(변경할 부분 선택).val(값);
    값을 가져 올 때 : $(변경할 부분 선택).val();




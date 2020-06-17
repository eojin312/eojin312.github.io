---
title: "html 에서 img 가 null 일 때 예외처리"     
date: 2020-06-17 13:20:28 -0400
categories: 공부
---

img 의 경로가 잘못되서 이미지가 엑스박스로 보일 때 많은 예외처리가 있습니다.
다른 이미지로 처리하거나, 아예 안보이게 해주면 됩니다.

**null 일때 안보이게 해주기**
```html
<div>
    <img id="file-name" onerror="this.parentNode.style.display='none'"/>
</div>
```

**null 일때 다른 이미지 보여주기**
```html
<div>
    <img id="file-name" onerror="this.src='에러발생이미지';" src="원본이미지">
</div>
```


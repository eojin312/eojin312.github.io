---
title: "css 적용 방법"     
date: 2020-08-09 14:20:28 -0400
categories: 공부
---

html 에 css 를 적용 시키는 방법
## inline
html 태그의 style attribute 로 정의해서 css 를 적용시킴

```html
<p style="color: #00bf8f"></p>
```

## internal
style 태그로 css 를 적용 시킴

```html
<style>
h1 {
    color: red;
}
</style>
```

## linking
별도 css 파일을 만들고 include 하는 방식

**page.css**
```css
h1 {
    color: red;
}
```

```html
<link rel="stylesheet" href="page.css">
```
---
title: "https"
date: 2020-01-31 13:48:28 -0400
categories: 개인공부
---

# Bootstrap

Bootstrap 은 **웹프로젝트 개발을 위한 가장 인기있는 HTML, CSS, JS 프레임워크**입니다. 기존에 짜여져있는 CSS, JS 에 데이터를 넣으면 쉽고 빠르게 사이트를 만들 수 있습니다.

## Bootstrap 사용 방법

Bootstrap 사이트에서 다운을 받으면 기본적인 CSS 와 JS 를 확인할 수 있습니다. 그럼 기본적으로 짜여져있는 HTML 에 Bootstrap CSS 경로와 Bootstrap JS 경로를 잡아주고
id 나 class 에 알맞은 변수를 넣어주면 됩니다. 좀 더 이쁜 사이트를 원한다면 https://startbootstrap.com/themes/sb-admin-2 과 같은 테마 사이트에서 압축파일을 다운받아서
css 와 js 를 연결해주면 됩니다. 저는 **sb-admin2** 를 사용했습니다. 그럼 적용을 위해서 기본적으로 해줘야할 코드들이 있습니다.
 
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="description" content="">
    <meta name="author" content="">
    
    <title>제목</title>
    
    <link rel="stylesheet" href="/static/bootstrap/theme/sbadmin2/all.min.css" type="text/css">
    <link rel="stylesheet" href="/static/bootstrap/theme/sbadmin2/sb-admin-2.min.css" type="text/css">
    
    <!--여기까지 head-->
    
    <!--여기서부턴 body 영역-->
    <script src="/static/bootstrap/theme/sbadmin2/jquery.js"></script>
    <script src="/static/bootstrap/theme/sbadmin2/bootstrap.bundle.js"></script>
    <script src="/static/bootstrap/theme/sbadmin2/jquery.easing.min.js"></script>
    <script src="/static/bootstrap/theme/sbadmin2/sb-admin-2.js"></script>
</head>
</html>
```

이런 코드를 적용하고 나면 보기 좋은 bootstrap 테마가 적용됩니다.

![스크린샷 2020-01-24 오후 9 22 58](https://user-images.githubusercontent.com/45488643/73068946-d6331800-3eef-11ea-8caf-b24978d58367.png)
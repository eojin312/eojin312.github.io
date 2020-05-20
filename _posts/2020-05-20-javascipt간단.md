---
title: "javascript 문자열에 변수 넣기"     
date: 2020-05-16 13:20:28 -0400
categories: 공부
---

자바스크립트에서 문자열에 변수를 넣을 수 있는 방법은 간단합니다!

평소 문자열이면 '' 를 쓸텐데 변수를 사용하려면 alt + ~ 를 사용해 `` 를 사용해야합니다
후에 ${} 안에 변수를 선언하면 됩니다.

```javascript
validate : function () {
    if (this.userDto.loginId == '') {
       alert('아이디를 입력해주세요');
       $('#loginId').focus();
       return false;
  }
};
```
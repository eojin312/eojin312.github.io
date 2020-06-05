---
title: "Required request part 'file' is not present"     
date: 2020-06-05 13:20:28 -0400
categories: 공부
---

## Required request part 'file' is not present

springboot + ajax + Mulitpart 를 이용해서 파일 업로드 기능을 만들고 회원 가입에 프로필사진 기능 쪽에 적용시키니 "Required request part 'attach-file' is not present" 라는 에러가 떴습니다

구글링을 해보니 많은 방법들이 있었습니다.
 
ajax 에서 content-type 을 제거하라는 방법 -> 이러면 current request is not a multipart request 에러가 발생합니다 (더 심각)

Controller 에서 @RestController 를 선언하라는 방법 -> 이미 선언되어있음에도 에러가 발생했습니다.

application.yml 에 multipart 설정을 하라는 방법 -> 아무 변화 없었습니다.

_하지만!!!!_   문제의 원인은 정말 가까운 곳에서 발견할 수 있었습니다.

```html
<form>
<div class="container">
    <div class="create">
        <div class="form-group">
            <label>이름</label>
            <input class="form-control" id="name" placeholder="이름을 입력해주세요" type="text">
        </div>
        <!--중략-->
        <div class="img-area">
            <label>프로필사진</label>
            <form action="/api/upload" enctype="multipart/form-data" id="upload-form" method="post" name="upload-form">
                <input name="attach-file" type="file">
                <input onclick="UserService.upload();" type="button" value="upload">
            </form>
        </div>
</form>
```
원인 찾으셨나요? 바로 form 안에 form 을 또 사용해서 spring 이 file 을 찾을 수 없었던 것입니다.
이럴 때 저같이 바보처럼 하루종일 찾아보는 방법도 있지만 더 효율적인 방법이 있습니다.

**브라우저에서 검사를 누르시면 브라우저가 해석한 Html 코드가 나옵니다.
form 을 못찾았다면 브라우저에서도 중복된 form 을 알아서 지워줍니다.**
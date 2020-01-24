---
title: "sitemesh 설정 + Bootstrap 사용"
date: 2020-01-24 20:43:28 -0400
categories: 개인공부
---

#Sitemesh 설정, 사용

> 학사관리 프로그램을 개발하는데 항상 사이트의 기능에만 신경썼지 view 는 신경쓰지않았습니다. 그러다보니 너무 부실하다는 생각이 들어서 html 과 css 를 통해 자체로 하기엔 너무 시간이 오래
걸릴 것 같아서 Bootstrap 을 사용하고 중복되는 코드를 sitemesh 를 사용해서 제거했습니다.

## Bootstrap

Bootstrap 은 **웹프로젝트 개발을 위한 가장 인기있는 HTML, CSS, JS 프레임워크**입니다. 기존에 짜여져있는 CSS, JS 에 데이터를 넣으면 쉽고 빠르게 사이트를 만들 수 있습니다.

## Bootstrap 사용 방법

Bootstrap 사이트에서 다운을 받으면 기본적인 CSS 와 JS 를 확인할 수 있습니다. 그럼 기본적으로 짜여져있는 HTML 에 Bootstrap CSS 경로와 Bootstrap JS 경로를 잡아주고
id 나 class 에 알맞은 변수를 넣어주면 됩니다. 좀 더 이쁜 사이트를 원한다면 https://startbootstrap.com/themes/sb-admin-2 과 같은 테마 사이트에서 압축파일을 다운받아서
css 와 js 를 연결해주면 됩니다. 저는 sb-admin2 를 사용했습니다. 그럼 적용을 위해서 기본적으로 해줘야할 코드들이 있습니다.
 
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
그렇게 된다면

![스크린샷 2020-01-24 오후 9 22 58](https://user-images.githubusercontent.com/45488643/73068946-d6331800-3eef-11ea-8caf-b24978d58367.png)

이런식으로 테마를 적용할 수 있습니다.

> 또 하나의 문제점이 생겼습니다. left-bar 와 top-bar 를 항상 모든 사이트에 공통적으로 사용하고싶었습니다. 그래서 무식하게 일단 모든 복잡한 테마 코드를 일일이 복사 + 붙여넣기를 반복했습니다. 하지만 left-bar 의 간단한 수정에
많은 코드들의 영향을 끼치니 너무 번거로웠습니다. java 처럼 관심사 분리를 해서 한 작업에만 집중하고 싶은 마음이었습니다. 

## Sitemesh

여러가지 프레임워크가 있지만 저는 sitemesh 가 좀 더 적용하기 쉬워보여서 사용했습니다.
Sitemesh 란 **웹페이지의 동일한 상단, 하단, 메뉴 등의 부분들은 한곳에서 관리하고 각각의 페이지는 실제 내용만을 관리하는 프레임워크입니다.** 

## Sitemesh 적용 + 사용방법

1. pom.xml 에
 
```xml
        <dependency>
            <groupId>opensymphony</groupId>
            <artifactId>sitemesh</artifactId>
            <version>2.4.2</version>
        </dependency>
```
를 적용합니다.

--> web.xml 에  
```xml
    <filter>
        <filter-name>sitemesh</filter-name>
        <filter-class>com.opensymphony.sitemesh.webapp.SiteMeshFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>sitemesh</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```
를 해서 Request 가 DispatcherServlet 에 도달하기 전 무조건 sitemesh 를 거치게 합니다. (url-pattern이 /* 인 이유는 어떤 하나라도 남기지말고 다 거치라는 뜻입니다.)

--> 여기서 중요합니다. WEB-INF 바로 아래에 sitemesh.xml 를 만듭니다. (어떠한 파일 안에 넣지마세요) 그 후

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sitemesh>

    <!--decorator 결정 -->
    <property name="decorators-file" value="/WEB-INF/decorator.xml"/> <!-- 데코레이터 파일을 불러온다. 데코레이터 경로 설정 --> 
    <excludes file="${decorators-file}"/>

    <!--parser를 설정:해당 페이지의 content-type이 뭐라고 쓰여있는지에 따라 데코레이터 결정 -->
    <page-parsers>
        <parser content-type="text/html"
                class="com.opensymphony.module.sitemesh.parser.HTMLPageParser"/>
        <parser content-type="text/html;charset=UTF-8"
                class="com.opensymphony.module.sitemesh.parser.HTMLPageParser"/>
    </page-parsers>

    <decorator-mappers>
        <mapper
                class="com.opensymphony.module.sitemesh.mapper.PrintableDecoratorMapper">
            <param name="decorator" value="printable"/>
            <param name="parameter.name" value="printable"/>
            <param name="parameter.value" value="true"/>
        </mapper>


        <mapper class="com.opensymphony.module.sitemesh.mapper.PageDecoratorMapper">
            <param name="property" value="meta.decorator"/>
        </mapper>

        <!-- mapper결정 : 설정파일 사용하여 decorator정하기 -->
        <mapper
                class="com.opensymphony.module.sitemesh.mapper.ConfigDecoratorMapper">
            <param name="config" value="${decorators-file}"/>
        </mapper>
    </decorator-mappers>
</sitemesh>
```
를 붙여 넣어줍니다. 

--> sitemesh.xml 처럼 decorator.xml 을 WEB-INF 바로 밑에 만들어줍니다.(위치 상관 없음 sitemesh.xml 에 자신이 설정한 경로대로 하면 된다.)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<decorators defaultdir="/WEB-INF/sitemesh/template"> 
    <decorator name="default" page="default.jsp">
        <pattern>/*</pattern>
    </decorator>
</decorators>
```
무조건 defaultdir 로 뒤에 할 default.jsp 의 경로를 잡아주어야합니다. 그리고 모든 jsp 에 default.jsp 를 적용한다고 pattern 에 명시해줍니다.

--> default.jsp 에선 sitemesh 를 적용하기위한 해야할 일이 있습니다.

```jsp
<?xml version="1.0" encoding="UTF-8" ?>
<%@ taglib uri="http://www.opensymphony.com/sitemesh/decorator" prefix="decorator" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE>
<html>
    <head>
        HEADER
        <decorator:head />
    </head>
        <body>
            BODY
            <decorator:body />
        </body>
</html>
```
적용 끝!

## sitemesh 말고 무엇이 있나?

기본적으로는 타일즈, 타임루프가 있습니다. 제가 비교하고싶은 건 sitemesh VS tiles 입니다.

> tiles sitemesh보다 어떤 장점을 가지고있을까

장점은 재사용성이 매우 높습니다.

> 나는 왜 tiles 를 안사용하고 sitemesh 를 선택했을까?

tiles 는 사용하는 사람마다 다르겠지만 초기 설정이 sitemesh 에 비해 어렵습니다. 저는 아직 새로운 기술을 능숙하게 다룰 수 있는 사람이 아닙니다. 그렇기에 설정하기 쉬운 sitemesh 부터 익혀보자라는 마음으로 사용하게 되었습니다.

> 최근에는 어떤 프레임워크를 사용할까?

spring boot 에서 사용하는 Time Leaf 를 사용합니다. Time Leaf 의 특징은 **다른 템플릿 엔진과 다르게 Html Tag를 그대로 사용한다는 것**입니다. 설정도 간단합니다. 프로젝트 생성 시 Time Leaf 를 선택하면 알아서 셋팅까지 다 해줍니다.


 


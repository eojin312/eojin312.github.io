---
title: "백기선님 DispatherServlet3부 viewResolver"
date: 2020-03-09 09:20:28 -0400
categories: 개인공부
---

DispatherServlet 에는 ViewResolver() 메소드가 있는데
InternalResourceViewResolver 빈 객체에 의해 
prefix + 뷰 이름 + suffix 에 해당하는 경로를 뷰 코드로 사용하는
InternalResourceView 타입의 View 객체를 리턴한다.

만약 빈에 등록된 것이 없다면? -> 그러면 기본전략으로 가져온다.

기본전략이란 건 어디에 정의되어있는데? -> DispatherServlet.properties 에 정의 해놨다.

<img width="1278" alt="스크린샷 2020-03-09 오전 9 39 48" src="https://user-images.githubusercontent.com/45488643/76174343-20473300-61ea-11ea-90d9-454f53f6141e.png">

그럼 직접 빈에 등록해보자!

```java
@Bean
public ViewResolver viewResolver() {
    InternalResourceViewResolver irvr = new InternalResourceViewResolver();
    irvr.setPrefix("/WEB-INF");
    irvr.setSuffix(".jsp");
}
```
setPrefix 는 지정한 경로 밑의 파일들을 가져와주고, setSuffix 는 .jsp 로 끝나는 파일들을 찾아와준다.
이렇게 해주면 굳이 Controller 에서 return "/WEB-INF/sample.jsp" 라고 번거롭게 치지않아도된다
ViewResolver가 알아서 다 해주기 때문!

다른 방법은 mvc-config 에 이렇게 직접 명시해두면 된다. 저는 이 방식을 사용했습니다
```xml
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/view"/>
        <property name="order" value="1"/>
        <property name="suffix" value=".jsp"/>
    </bean>
```
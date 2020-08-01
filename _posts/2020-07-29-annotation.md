---
title: "커스텀 어노테이션과 메타 어노테이션"     
date: 2020-07-29 13:20:28 -0400
categories: 공부
---

> 코딩을 하다보면 중복되는 코드도 많아지고, 읽기 어렵고 이 메소드 혹은 이 어노테이션이 무슨 역할을 하고있는 녀석인지 감을 못잡고 코딩이 점점 더 꼬여질 때가 있을 것입니다.
>자바는 이를 막기위해서 커스텀 어노테이션을 java5 이후부터 도입했습니다.

# 어노테이션

@를 이용한 주석, 자바코드에 주석을 달아 특별한 의미를 부여한 것

# 커스텀 어노테이션
인터페이스 껍데기를 어노테이션 형식으로 만드는 걸 말합니다. 클래스, 필드, 메서드 같은 곳에 붙이면 어노테이션으로써 껍데기 역할은 할 수 있게 됩니다. 스프링에서 제공하는 어노테이션이 아닌 자신이 직접 어노테이션을 만들어 사용합니다.

**어쩔 때 사용하나요?**
 - 중복되는 코드들이 반복될 때
 - 코드를 간결하게 하고싶을 때
 - 의미도 더욱 구체적으로 하고싶을 때
 - 코드의 가독성을 높히고싶을 때

밑에 메타 어노테이션에 관하여 설명하겠지만 여기서 잠깐 설명하자면 @Target, @Retention 어노테이션은 커스텀 어노테이션을 만들 때 꼭 필요한 설정정보 어노테이션입니다. 
```java
@Documented
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@RequestMapping(method = RequestMethod.GET, value = "/hello")
public @interface GetHelloMapping {
}
```

## 메타 어노테이션

어노테이션에 사용할 수 있는 어노테이션
어노테이션의 범위나 어떤 형식에 맞게 사용하게 해주는지 기본 설정들 혹은 추가해주고픈 어노테이션 기능들을 추가합니다.

밑 예제는 중복되는 @RequestMapping("/hello") 를 따로 빼서 메타 어노테이션으로 바꾼 것입니다.
```java
public class SampleController {
    @GetHelloMapping
    @ResponseBody
    public String annotation() {
        return "hello";
    }
}
```

## 메타 어노테이션 종류
 1. @Retention
    
    - 해당 어노테이션 정보를 언제까지 유지할 것인가

커스텀 어노테이션을 만들 때 @Retention 으로 범위를 정해주지않으면 런타임시 소멸됩니다.

```java
@Retention(RetentionPolicy.RUNTIME)
@Retention(RetentionPolicy.CLASS) 
@Retention(RetentionPolicy.SOURCE)
```
 - RUNTIME => 컴파일 이후에도 JVM이 참조 가능합니다.
 - CLASS => 컴파일러가 클래스를 참조할 때까지만 유효합니다.
 - SOURCE => 이 어노테이션은 컴파일 이후 사라집니다. 
 
 2 . @Target
 
 - 어노테이션을 어떤 요소에 적용할지 지정
 
```java
@Target(ElementType.ANNOTATION_TYPE)
@Target(ElementType.METHOD)
@Target(ElementType.FIELD)
..등등..
```
타겟에 설정한 범위에서만 사용가능한 어노테이션을 정의할수있다

## 컴포스트(조합) 어노테이션
한 개 혹은 여러 개의 메타 어노테이션을 조합해서 만든 커스텀 어노테이션. 이건 이미 조합된 어노테이션이라 커스텀 어노테이션에 사용 못합니다.

이건 조합 어노테이션 예제입니다. controller 에서 자주 사용하는 @GetMapping 은 조합 어노테이션이기에 커스텀 어노테이션에선 사용 못합니다.
커스터마이징을 위해 메타 어노테이션의 속성들을 재선언할 수 있습니다. 이런 특징을 이용하면 메타 어노테이션 속성들의 일부분만 노출하고 싶은 경우에 유용하게 사용할 수 있습니다.
그리고 어노테이션의 강점인 코드를 간결하게 줄일 수 있고, 보다 구체적인 의미를 부여할 수 있습니다.
```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@RequestMapping(
    method = {RequestMethod.GET}
)
public @interface GetMapping {
    @AliasFor(
        annotation = RequestMapping.class
    )
..생략..
```

# 조합 어노테이션

 한 개 혹은 여러개의 어노테이션을 조합하여 만든 어노테이션
 
  커스터마이징을 위해 메타 어노테이션의 속성들을 재선언할 수 있습니다. 이런 특징을 이용하면 메타 어노테이션 속성들의 일부분만 노출하고 싶은 경우에 유용하게 사용할 수 있습니다.
```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@RequestMapping(
    method = {RequestMethod.GET}
)
public @interface GetMapping {
    @AliasFor(
        annotation = RequestMapping.class
    )
    String name() default "";
..중략..
```

## 결론
- 메타 어노테이션과 조합 어노테이션을 사용하면 코드가 간결해진다.
- 어노테이션을 통해 코드의 난이도가 낮아진다.
- 어노테이션으로 재선언이 가능하다


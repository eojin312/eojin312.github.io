---
title: "커스텀 어노테이션"     
date: 2020-07-29 13:20:28 -0400
categories: 공부
---

> 코딩을 하다보면 중복되는 코드도 많아지고, 읽기 어렵고 이 메소드 혹은 이 어노테이션이 무슨 역할을 하고있는 녀석인지 감을 못잡고 코딩이 점점 더 꼬여질 때가 있을 것입니다.
>자바는 이를 막기위해서 커스텀 어노테이션을 java5 이후부터 도입했습니다.

# 어노테이션

@를 이용한 주석, 자바코드에 주석을 달아 특별한 의미를 부여한 것

# 메타 어노테이션

어노테이션에 사용할 수 있는 어노테이션

**어쩔 때 사용하나요?**
 - 중복되는 코드들이 반복될 때
 - 코드를 간결하게 하고싶을 때
 - 의미도 더욱 구체적으로 하고싶을 때
 - 코드의 가독성을 높히고싶을 때

밑 예제는 중복되는 @RequestMapping("/hello") 를 따로 빼서 메타 어노테이션으로 바꾼 것입니다.
```java
@RequestMapping(method = RequestMethod.GET, value = "/hello")
public @interface GetHelloMapping {
}

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
    
    - 어떤 시점까지 어노테이션이 영향 미치는 지 결정합니다.
    
```java
@Retention(RetentionPolicy.RUNTIME)
@Retention(RetentionPolicy.CLASS) 
@Retention(RetentionPolicy.SOURCE)
```
 - RUNTIME => 컴파일 이후에도 JVM이 참조 가능합니다.
 - CLASS => 컴파일러가 클래스를 참조할 때까지만 유효합니다.
 - SOURCE => 이 어노테이션은 컴파일 이후 사라집니다. 
 
 2. @Target
 
 -> 공부 좀 더 하고 보강할 예정입니다

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


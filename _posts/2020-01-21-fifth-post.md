---
title: "제네릭 타입"
date: 2020-01-21 22:42:28 -0400
categories: 개인공부
---

# 제네릭 타입

> 저는 "JAVA 프로그래밍 면접 이렇게 준비한다" 라는 책을 보며 공부합니다. 오늘은 4장 리스트에 관하여 공부하고 있었습니다. 하지만 문득 List 타입이 궁금해서 commend + 클릭 을 통해 구현 클래스를 들어갔고
List 의 interface 인 Collection 클래스에 들어가게 되고 Collection 의 interface 인 Iterator 를 보게 되었습니다. 그 중 <? super T> 라는 제네릭을 발견했습니다.

## 제네릭이란

제네릭은 **클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법을 의미** 합니다. List<String> stringList 라고 타입을 지정했다면 String 타입으로밖에 사용할 수 없습니다.

> 제네릭이 없었던 자바 버전에선 어떻게 사용했을까?

제네릭은 java5 부터 사용하기 시작했습니다. 그전에 List 의 타입은 모두 Object 였습니다. 즉 0번째 값이 int 1 인지 String '1' 인지는 배열에 값을 넣은 사람만 알 수 있었던 것입니다.

```java
package hachi.playground;

import hachi.education_management.student.model.Student;

import java.util.ArrayList;
import java.util.List;

public class ListTest {

    public void listTest(){
        List<Object> list = new ArrayList<>();

        list.add(1);
        list.add("2");

        Student s = (Student) list.get(0);

    }
}
```
그렇기에 Student 타입으로 사용하려면 꼭 캐스팅을 해야하는 번거로움이 있었습니다.

> 위에서 언급했던 <? super T>  는 뭘까?
 
T 는 Type 을 뜻합니다. 
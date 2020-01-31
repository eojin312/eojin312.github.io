---
title: "generic-type"
date: 2020-01-21 22:42:28 -0400
categories: 개인공부
---


## 제네릭이란

제네릭은 **클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법을 의미** 합니다. 
예를 들어 List<String> stringList 라고 타입을 지정했다면 String 타입으로밖에 사용할 수 없습니다.

> 제네릭이 없었던 자바 버전에선 어떻게 사용했을까?

제네릭은 java5 부터 사용하기 시작했습니다. 그전에 List 의 타입은 모두 Object 였습니다. 즉 0번째 값이 int 1 인지 String '1' 인지는 배열에 값을 넣은 사람만 알 수 있었던 것입니다.
그렇기에 Integer 타입으로 사용하려면 꼭 캐스팅을 해야하는 번거로움이 있었습니다.

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

        Integer i = (Integer) list.get(0);

    }
}
```
혹시나 이해가 안될까봐 head first java 에서 나오는 쉬운 설명을 넣었습니다.

![KakaoTalk_Photo_2020-01-31-20-38-54](https://user-images.githubusercontent.com/45488643/73536690-d1c7ab80-4469-11ea-8289-eaf511142cd6.jpeg)

## 회고점
항상 List 를 사용할 때 <> 안에 들어가있는 것이 타입인 줄은 알았지만 클래스 타입이여야 하고 
오로지 그 타입 외 다른 타입은 사용 안된다는 점을 알게되었습니다. 아직 부실한 내용인 것 같아 나중에 점점 공부하면서 채워나갈 생각입니다.  
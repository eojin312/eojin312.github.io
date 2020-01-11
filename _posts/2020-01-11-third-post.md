---
title: "Property not found on type"
date: 2020-01-11 13:54:28 -0400
categories: 회고
---

학사관리 프로그램을 열심히 코딩하는 중에 Controller 에서 JSP(View) 로 데이터를 넘기는 일을 했습니다. 하지만 **Property not 'examNo' found on type** 이라는 에러메시지와 함께
프로그램이 돌아가지 않았습니다. <br>
 
## 디버그 해보기
리스트에 데이터는 잘 담겼는지 확인하기 위해 Controller 에 디버깅을 걸어보았습니다. 
![debug-capture](https://user-images.githubusercontent.com/45488643/72199781-e96fcd80-3483-11ea-8235-4f7d0d35f5f9.jpg)
분명 examList 이름도 문제 없고, jsp 에 데이터 전달도 잘 되었을텐데.. 
jsp 에서는 examNo 라는 Property 를 못찾는게 이해가 되지않았습니다.

## Vo 확인하기
데이터에 잘 담겼다면 Controller 의 문제가 아닌 걸로 확정 짓고, 데이터를 담아주는 Vo 에서 문제가 있는 지 확인해보았습니다.

![vo](https://user-images.githubusercontent.com/45488643/72199786-fab8da00-3483-11ea-9d9e-e29697ff208d.jpg)
<br>하지만 examNo는 너무 당당하게 선언되어있었습니다 이도 저도 생각해봐도 모르겠어서 구글링을 했습니다.
에러 메시지는 같지만 다른 케이스가 있었습니다.

![capture](https://user-images.githubusercontent.com/45488643/72199792-1b812f80-3484-11ea-9348-d67e9462d73e.jpg)

이런 케이스도 경험했긴했지만, 지금은 이 케이스가 아닙니다.
그래도 이 케이스도 삽질 않기위해 지금 한번 더 머리속에 쏙쏙 넣어두었습니다.
<br>
출처: (https://limeeyojung.tistory.com/15)

## 구글링 하기
그런데 검색결과 중 스택오버플로우의 답변에서 아차 싶었다는 걸 발견!!!
주저리주저리 길게 영어로 친절히 써있지만 요약하자면,
 jsp 내에서 프로퍼티 참조는 **getter** 고, 이름규칙(네이밍 컨벤션)은 
메서드 이름이 get으로 시작하는(접두어가 get인) public 메소드여야하고
다만, return type이 boolean 일 경우에는 is~~ 여야한다..라는 답변이었습니다.
![capture2](https://user-images.githubusercontent.com/45488643/72199802-3784d100-3484-11ea-81c8-7e3504528d58.jpg)

스택오버플로우의 내용을 토대로 examNo Property (= 클래스의 멤버변수) 에 대해 getter ∴ public getExamNo() 가 존재하는지 확인해보았습니다.

![getter](https://user-images.githubusercontent.com/45488643/72199809-4a97a100-3484-11ea-9f66-149cd919d7c2.jpg)

이런;; getter 생성에 examNo 가 있다는 것은 examNo 에 대한 getter 가 없다는 것이죠..

그래서 곧장 getter를 생성했습니다.

![examNo-getter](https://user-images.githubusercontent.com/45488643/72199812-63a05200-3484-11ea-8654-b8b6efdd2ce7.jpg)

그 후 결과를 확인해보니

![result](https://user-images.githubusercontent.com/45488643/72199814-6f8c1400-3484-11ea-98e6-667e56242b10.jpg)


## 시사점
1. jsp에서 프로퍼티 접근은 변수로 직접 접근하는게 아니라, getter 를 통해 호출된다는걸 잊고 있었다
    - 더불어  boolean 형은 is~~~()를 호출하는 것도 잊지 말아야..
2. jsp도 결국 servlet class로 변환되고, $(exam.examNo} 부분은 실제로 out.print(exam.getExamNo());형태로 바뀌어 동작할 것이다. 
3. private int examNo 는 private 이라 외부 클래스에서 접근이 당근 불가능하다 **(캡슐화되어있기때문에)**

## 에러 해결 후 관련 책 찾기

에러를 해결 했다고 끝이 아닙니다. HEAD FIRST - Servlet & JSP 책을 찾아보고 
앞으로 이런 에러가 발생했을 때 수 많은 해결방법을 시도하다가 시간을 허비하지말고 
몇 가지 중점적인 해결방법으로 단 기간에 해결해야합니다.

![jsp-servlet](https://user-images.githubusercontent.com/45488643/72199818-8df20f80-3484-11ea-8a01-2d62e692d7cc.jpg)

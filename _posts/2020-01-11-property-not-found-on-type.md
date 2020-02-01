---
title: "Property not found on type"
date: 2020-01-11 13:54:28 -0400
categories: 회고
---

Controller 에서 JSP(View) 로 데이터를 넘기는 과정에서 
**Property not 'examNo' found on type** 이라는 exception이 발생했습니다.
 
## 디버깅 해보기
JSP에 넘겨줄 리스트 변수에 데이터는 잘 담겼는지 확인하기 위해 Controller 에 브레이크포인트를 걸어보았습니다. 
![debug-capture](https://user-images.githubusercontent.com/45488643/72199781-e96fcd80-3483-11ea-8235-4f7d0d35f5f9.jpg)
분명 examList 라는 변수명에 오타도 없고,  
jsp 에서는 examNo 라는 항목을 못찾겠다고 하니 정말 난감했습니다.

## 모델 확인하기
데이터에 잘 담겼다면 Controller 의 문제가 아닌 걸로 확정 짓고, 데이터를 담아주는 모델에 문제가 있는 지 확인해보았습니다.
![vo](https://user-images.githubusercontent.com/45488643/72199786-fab8da00-3483-11ea-9d9e-e29697ff208d.jpg)
모델에 examNo는 멤버변수로 떡하니 존재했습니다. 너무 당황스러워서 마구 구글링했습니다.
에러 메시지는 동일하지만 다른 케이스가 발견되긴 했습니다.
![capture](https://user-images.githubusercontent.com/45488643/72199792-1b812f80-3484-11ea-9348-d67e9462d73e.jpg)

이런 케이스도 경험했긴했지만, 지금은 이 케이스가 아닙니다.   
그래도 이 케이스도 삽질 않기위해 지금 한번 더 머리 속에 일단 킵해두었습니다.
<br>
출처: (https://limeeyojung.tistory.com/15)

## 나올떄까지 구글링
그런데 검색결과 중 스택오버플로우에서 오아시스를 발견!
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

코드를 수정하고 다시 실행!! 와 된다!!
![result](https://user-images.githubusercontent.com/45488643/72199814-6f8c1400-3484-11ea-98e6-667e56242b10.jpg)


## 시사점
1. jsp에서 프로퍼티 접근은 변수로 직접 접근하는게 아니라, getter 를 통해 호출된다는걸 잊고 있었다
    - 더불어  boolean 형은 is~~~()를 호출하는 것도 잊지 말아야..
2. jsp도 결국 servlet class로 변환되고, $(exam.examNo} 부분은 실제로 out.print(exam.getExamNo());형태로 바뀌어 동작할 것이다. 
3. private int examNo 는 private 이라 외부 클래스에서 접근이 당근 불가능하다 **(캡슐화되어있기때문에)**

## 에러 해결 후 관련 책 찾기
다시금 기초의 중요성을 알게되었습니다. 헤드퍼스트 servlet&JSP책에서 해당 부분을 다시 확인하고 머리 깊숙히 넣어두었습니다.
![jsp-servlet](https://user-images.githubusercontent.com/45488643/72199818-8df20f80-3484-11ea-8a01-2d62e692d7cc.jpg)

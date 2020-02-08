---
title: "git branch"
date: 2020-02-08 17:03:28 -0400
categories: 개인공부
---


브랜치 = 나뭇가지
 
## 뜻 
작업을 분기해서할 수 있는 것!

## 언제 사용하나 
안정적인 작업과 안정적이지 않은 작업을 진행해야할 때 사용한다.

## 브랜치 생성
>git branch [branch name]

혹은 sourceTree 를 사용한다면 우클릭을 눌러 새 브렌치를 누르면 된다

 ![스크린샷 2020-02-08 오후 5 45 16](https://user-ima ges.githubusercontent.com/45488643/74082172-d3f9bd80-4a9a-11ea-95c4-a32b28421ddc.png)
 
## 변경

> git checkout [branch name]
 
## 병합 (merge)
브랜치는 똑같은 파일을 따로 나누어서 하기 때문에 브랜치를 따서 따로 작업하면 master 의 코드와 달라지는게 당연하다
다시 master 코드에 적용해야하는데 그럴 때 merge 를 하면 된다

## 충돌해결 (conflict)

각자 다른 브랜치에서 같은 부분을 수정했다면 master 에서 충돌이 난다. git 은 어떤 코드를 선택할 지 모르기에
사람에게 권한을 위임해주는 것


```java
public abstract class Human {
    protected String name;
    protected String head;
    protected String arm;
    protected String leg;

    protected String getName() {
        return name;
    }

    protected String getHead() {
        return head;
    }
    
====
    protected String getArm() {
        return arm;
    }
====
    protected String getLeg() {
        return leg;
    }
}
```

=== 으로 변경된 두 부분을 가지고오고 필요한 걸 제외하고 지우면된다.



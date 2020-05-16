---
title: "intellij 생성자 에러"     
date: 2020-05-16 13:20:28 -0400
categories: 공부
---

엔티티에서 Builder 패턴을 사용하기위해 생성자를 자동 생성하려고했습니다.

하지만 이런 에러가 생기고 말았습니다.. 전 분명 많은 컬럼들을 생성해놨는데 하나도 자동으로 생성되지않았습니다.
```java
    @Builder
    public User() {
    }
```

여러가지 구글링 끝에 최신 intellij 버전에서 이미 문제가 생겨서 해결하는 중에 있다고 합니다ㅠㅠ

[https://youtrack.jetbrains.com/issue/IDEA-229354?_ga=2.232878558.1153493747.1589606116-1967576558.1587529313]

## 원인

@Entity 를 먼저 선언해서 에러가 생긴 것이라고합니다.
일단 @Entity 를 지우고 생성자를 만든 다음 다시 추가해야할 것 같습니다ㅠ

```java
    @Builder
    public User(String name, String email, String loginId, String loginPassword, String profileImage, int birthYear, String gender) {
        this.name = name;
        this.email = email;
        this.loginId = loginId;
        this.loginPassword = loginPassword;
        this.profileImage = profileImage;
        this.birthYear = birthYear;
        this.gender = gender;
    }
```

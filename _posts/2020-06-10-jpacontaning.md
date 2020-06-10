---
title: "jpa containning"     
date: 2020-06-10 13:20:28 -0400
categories: 공부
---

## jpa containning

sql 언어로 LIKE 역할을 해줍니다

JpaRepository 를 상속받은 repository interface 에서 메소드를 선언해주고 service 에서 사용하면 됩니다.

**UserRepository**
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Page<User> findByLoginIdContaining(String searchKeyword, Pageable pageable);
}
```
저는 제 프로젝트의 검색 기능을 만들어주기 위해서 loginId 용으로 검색하는 메소드를 선언했습니다.
검색해서 나온 결과물들이 보기 좋게끔 페이징 처리까지 해줬습니다.

꼭 사용할 때는 형식을 지켜줘야합니다.
 
    findBy + 필터 조건  + Containning
   
 


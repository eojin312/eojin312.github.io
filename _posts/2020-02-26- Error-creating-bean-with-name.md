---
title: "Error creating bean with name"
date: 2020-02-26 18:50:28 -0400
categories: 회고
---

>프리렉 게시판으로 간단한 게시글 리스트를 만들던 중 **Error creating bean with name** 라는 에러 메시지가 뜨며 스프링이 실행되지 않았습니다.
이것저것 저의 생각으로 이것저것 바꿔보고, service 에 @Service 가 잘 선언되어있나 확인했지만 어떤 문제도 있지않았습니다.
결국 구글링을 해보니 repository 에 있는 쿼리 문제라는 것을 알아냈습니다.

처음 문제 발생지점 솔직히 빨간 줄도 없고 jpa 쿼리를 처음 접하다 보니 문제를 발견할 수 없었습니다.
```java
public interface PostsRepository extends JpaRepository<Posts, Long> {

    @Query("SELECT p FROM posts p ORDER BY p.id DESC")
    List<Posts> findAllDesc();
}
```
스프링을 돌리면 중간에 멈추고 이런 에러 메시지가 나옵니다. 내용은 bean 을 생성할 수 없다는 내용입니다.
```java
Error starting ApplicationContext. To display the conditions report re-run your application with 'debug' enabled.
2020-02-26 18:46:29.800 ERROR 12260 --- [  restartedMain] o.s.boot.SpringApplication               : Application run failed

org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'postsService' defined in file [/Users/user/IdeaProjects/fl-springboot/target/classes/hachi/flspringboot/service/posts/PostsService.class]: Unsatisfied dependency expressed through constructor parameter 0; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'postsRepository': Invocation of init method failed; nested exception is java.lang.IllegalArgumentException: Validation failed for query for method public abstract java.util.List hachi.flspringboot.domain.posts.PostsRepository.findAllDesc()!
```

구글링을 하면 모두들 쿼리 문제라고 말씀하시는데 쿼리에 무슨 문제가 있는 지 알 수 없었습니다. 그렇게 해매다가 이 책 쓰신 분의 github 의 정답 코드를 보고 제가 직접 짠 코드와 비교해봤습니다.

```java
public interface PostsRepository extends JpaRepository<Posts, Long> {

    @Query("SELECT p FROM posts p ORDER BY p.id DESC") // 내가 짠 코드
    List<Posts> findAllDesc();
    
    @Query("SELECT p FROM Posts p ORDER BY p.id DESC") // 이동진 님 코드
        List<Posts> findAllDesc();
}
```
차이가 보이시나요? 단순한 대문자 소문자 오타였습니다. 얼핏 되게 멍청하고 단순한 문제라고 생각하지만 저는 이런 오타로 해맨 적이 많습니다. 물론 앞으론 더 꼼꼼하게 코드를 짜서 이런 문제를 발생하지 않게 해야하지만 저처럼 처음 jpa를 써보면서 익숙하지 않은 분들은 이런 문제 겪으실 것 같아 써봤습니다.

  
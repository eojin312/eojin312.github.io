---
title: "내가 경험하고 공부한 ManyToMany 끄적여보기"     
date: 2020-08-17 13:20:28 -0400
categories: 공부
---

>제가 경험했던 사례는 게시판의 카테고리 기능이었습니다. 카테고리 기능을 만들기전에 고민해봅시다. 간단하게 생각하자면 post 에 category 컬럼 하나 추가해서 
where 절에 카테고리 적용해서 리스트 뽑아내면 참 좋겠다고 생각하여 그렇게 구현해보려했습니다 하지만 다른 커뮤니티 사이트들의 카테고리는 그렇지않다는 걸 알게되었습니다
제가 생각했던 방법은 현실 서비스에선 상상할 수 없습니다..

## 설명

**에펨코리아 커뮤니티**

![스크린샷 2020-08-17 오후 1 33 48](https://user-images.githubusercontent.com/45488643/90357574-686a5b80-e08e-11ea-8c25-b3b96e4dda20.png)

만약에 윗 상황처럼 했다고 가정해봅시다. 시간이 지나서 게시판이 더 커지고 카테고리 종류도 많아졌습니다.
적어도 이렇게 변경해야할 것 같지않습니까? 이렇게 하려면 카테고리를 밖으로 꺼내서 분류시켜야합니다.. post 에는 게시글에 대한 
데이터만 있어야하는데 category 데이터가 더 많아지기 때문입니다.

**하나의 데이터는 한 곳에 있어야한다.** 정규화의 목적입니다. category 가 확장할 수록 따로 분류하는 건 정규화의 목적을 지키는 것입니다.
뭐요? 게시글을 추가할 때 category 도 추가해야하는데 insert 할 때 어쩌냐고요? 그럴 땐 윗 사진의 **category_mapping** 처럼 연결 테이블을 하나 두고 
foreigen key 만 모아두는 테이블로 서로 연결시켜주면 됩니다.

jpa 에 @ManyToMany 처럼 **다 대 다** 관계를 처리하는 어노테이션이 있습니다.
![jointable](https://user-images.githubusercontent.com/45488643/90357196-12e17f00-e08d-11ea-8871-a61482355549.png)

@ManyToMany 는 이런 식으로 우리가 설계했던 관계로 처리해줍니다. 하지만 @ManyToMany엔 여러 문제가 있는데 일단 코드로 봐봅시다!

## ManyToMany 로 관계 짓기(실제 예제 코드)

**Category**
```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
@Column(name = "category_id")
private Long id;

@ManyToMany(mappedBy = "categories")
private List<Post> posts = new ArrayList<>();
```

**Post**
```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
@Column(name = "post_id")
private Long id;

@ManyToMany
@JoinTable(name = "category_mapping", joinColumns = @JoinColumn(name = "post_id"), inverseJoinColumns = @JoinColumn(name = "category_id"))
private List<Category> categories = new ArrayList<>();
```

|코드|내용|
|------|---|
|@JoinTable|어노테이션 이름 그대로 join table (연결테이블) 을 굳이 엔티티 클래스를 만들지않아도 저절로 만들어줍니다.|

실행해봅시다. 잘 된걸 확인해볼 수 있습니다. 오잉 그러면 뭐가 문제라는 거죠???

![category](https://user-images.githubusercontent.com/45488643/90359010-b2554080-e092-11ea-8e7b-5ffa60173d26.png)

## 현실적인 고민들..

여기서부터 현실적인 문제를 고민하게됩니다. 그 두가지를 알려드리려합니다.

1. category_mapping 테이블엔 forigen key 말곤 추가 항목을 넣을 수 없습니다. 추가항목..
뭐가 있을까요 해보면, 누가 언제 카테고리를 추가했는지 예를 들어 create_date 같은 항목,
admin_name 같은 항목을 category_mapping 에 추가하지 못합니다.

2. 쿼리가 자신이 원하는대로 나오지 않습니다. 카테고리를 기준으로 게시글을 나열하는 쿼리를 짜면
이런 쿼리가 나옵니다.

```sql
SELECT p.id AS id1_0_0_,
       c.id AS id1_2_1_,
       p.title AS title2_0_0_,
       t.name AS name2_2_1_,
       c.post_id AS post_id1_1_0__,
       cm.category_id AS category_id2_1_0__
FROM   post p
INNER JOIN
       category_mapping cm
ON     p.id = cm.post_id
INNER JOIN
       category c
ON     cm.category_id = c.id
WHERE  p.id = ?
 
DELETE FROM post_category
WHERE  post_id = ?
 
INSERT INTO post_category
       ( post_id, category_id )
VALUES ( ?, ? )
```
밑에 보시면 Hibernate 가 모든 row 를 지운다음 나머지를 insert 하는 형태가 발생합니다.
이거 또한 해결방안이 있습니다 List 타입을 Set 타입으로 바꿔서 사용하면 되지만 저는 단순 쿼리가 맘에 안들어서
ManyToMany 를 버리는 것이 아니기 때문에 굳이 실험해보진 않겠습니다.

![unnamed](https://user-images.githubusercontent.com/45488643/90359900-1aa52180-e095-11ea-9f92-a48a70f43431.jpg)

## 다른 방법(실제 예제 코드)

방법은 많습니다. 꼭 ManyToMany 를 고집할 필요는 없죠.
jointable 을 따로 만들어서 하는 방법도 있습니다!

**Category**
```java
@OneToMany
@JoinColumn(name = "category_id")
private List<CategoryPost> categoryPosts;
```

**CategoryPost**
```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
@Column(name = "category_mapping_id")
private Long id;

@ManyToOne
@JoinColumn(name = "post_id")
private Post post;

@ManyToOne
@JoinColumn(name = "category_id")
private Category category;
```

**Post**
```java
@OneToMany
@JoinColumn(name = "post_id")
private List<CategoryPost> categoryPosts;
```

저 세 코드를 보곤 알겠나요?? 이해 안된다면 사진 한 장으로 설명해보겠습니다.

![relationship](https://user-images.githubusercontent.com/45488643/90372889-12a4ac00-e0ac-11ea-9e6a-0467465b8a5a.png)

짠! post_category_mapping 은 CategoryPost 입니다(실수로 이름을 못바꿨습니다..ㅎ)






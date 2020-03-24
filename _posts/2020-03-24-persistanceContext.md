---
title: "@PersistanceContext"
date: 2020-03-24 13:20:28 -0400
categories: 개인공부
---
Spring 에서 annotation 을 이용한 @Component 에 의해 등록되는 Bean 중 하나인 @PersistanceContext 입니다.



엔티티를 영구 저장하는 환경입니다.

 Entity Manager 로 엔티티를 저장 persist() ,조회( find(), JPQL , QueryDSL )
 **EntityManager 는 그 엔티티를 영속성 컨테스트에 보관하고 관리합니다.**
 
 ```java
    @PersistenceContext
    private EntityManager em;

    public void save(Member member) {
        em.persist(member);
    }
    
    public List<Member> findAll() {
        return em.createQuery("select m from Member m", Member.class)
                .getResultList();
    }

    public List<Member> findByName(String name) {
        return em.createQuery("select m from Member m where m.name = :name", Member.class)
        .setParameter("name", name)
        .getResultList();
    }
```

[참고](https://victorydntmd.tistory.com/207)
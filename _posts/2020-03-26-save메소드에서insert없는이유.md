---
title: "test save메소드에서 insert 하지않는 이유"
date: 2020-03-24 13:20:28 -0400
categories: 개인공부
---
jpa 를 사용하면서 save()메소드의 test 를 돌릴때 중요한 insert 는 안하고
select 만 하는 것을 확인할 수 있습니다
 
```sql
select
        member0_.member_id as member_i1_4_,
        member0_.city as city2_4_,
        member0_.street as street3_4_,
        member0_.zipcode as zipcode4_4_,
        member0_.name as name5_4_ 
    from
        member member0_ 
    where
        member0_.name=?
```

그 이유는 save 에서 persist 를 하면 
test 에서는 기본적으로 insert 를 안합니다
**트랜젝션 커밋이 될 때 insert 를 하기 때문입니다**

실제 실행될 때는 flush 메소드가 실행되면서 db 에 insert 가 한번에 찍히는데
테스트는 rollback 하기 때문에 영속성으로 커밋이 안됩니다.
rollback 을 하는 이유는 테스트이기 때문 기록을 남기면 에러가 생기기 때문입니다
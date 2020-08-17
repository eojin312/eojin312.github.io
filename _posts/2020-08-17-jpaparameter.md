---
title: "JPA 에 파라미터 정보 뜨게하기"     
date: 2020-08-17 13:20:28 -0400
categories: 공부
---
>JPA 를 사용하다보면 쿼리부분에 파라미터는 항상 ? 로 되어있는 걸 볼 수 있습니다.
? 안에 담긴 내용이 보여야 값이 제대로 저장되었는지 확인할 수 있는데 그러지못해서 불편했습니다.
그래서 이번에 파라미터 정보를 투명하게 보여줄 수 있는 방법을 알려드리려고합니다!

생각보다 간단합니다 application.yml 혹은 application.properties 에 설정 추가하시면 됩니다.

**application.yml**
```yaml
logging:
  level:
    org.hibernate.SQL: DEBUG
    org.hibernate.type: trace
```

yml 은 조심해야할 부분이 있습니다.
![applicationyml](https://user-images.githubusercontent.com/45488643/90372031-bd1bcf80-e0aa-11ea-8d6a-1e23e0b1e6fb.png)

logging 부분에 들여쓰기를 잘못하면 org.~~~:trace 이 동작안합니다.
**꼭 logging 부분을 들여쓰기하지말고 벽에 붙여야합니다!!**

**application.properties**
```properties
logging.level.org.hibernate.SQL=debug
logging.level.org.hibernate.type.descriptor.sql=trace
```

**결과물!**
```sql
Hibernate: 
insert 
into
    category
    (category_id, name) 
values
    (null, ?)
binding parameter [1] as [VARCHAR] - [유머]
```

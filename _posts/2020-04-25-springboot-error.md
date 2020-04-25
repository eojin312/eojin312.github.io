---
title: "SpringBoot 프로젝트 에러"
date: 2020-04-25 13:20:28 -0400
categories: 회고
---

```console
***************************

APPLICATION FAILED TO START

***************************

Description:
Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.
Reason: Failed to determine a suitable driver class 
```

데이터베이스 드라이버를 입력하라는 에러 메세지이다!
즉 application.property 에서 설정해주면 되는 것이다

```console
# DB설정
spring.datasource.driverClassName=com.mysql.jdbc.Driver

spring.datasource.url=jdbc:mysql://localhost:3306/quicksort?autoReconnect=true&amp;serverTimezone=UTC

spring.datasource.username=[mysql 아이디]

spring.datasource.password=[mysql password]
```
만약 JPA 를 사용한다면 application.property 에서 JPA 를 어떤 DB 를 사용할 지 정해준다

```console
#JPA 설정
spring.jpa.database-platform=org.hibernate.dialect.MySQL5Dialect
spring.jpa.show-sql=false
spring.jpa.hibernate.ddl-auto=create
``` 

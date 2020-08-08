---
title: "NoSuchMethodError JPQLSerializer.getConstantToAllLabels()Ljava 오류 (해결)"     
date: 2020-08-08 13:20:28 -0400
categories: 공부
---

![에러사진](https://user-images.githubusercontent.com/45488643/89704874-0d9c7a00-d993-11ea-8c74-3eecb01d7f57.png)

## 계기
Querydsl 을 사용하면서 테스트를 돌려봤는데 이런 에러가 발생했습니다. **NoSuchMethodError JPQLSerializer.getConstantToAllLabels()Ljava** 라는 에러인데 처음엔 검색에도 안나왔지만 stackoverflow 에서 간신히 해결방법을 찾았습니다.

[stackoverflow 에서 찾은 해결방법](https://stackoverflow.com/questions/16218100/querydsl-nosuchmethoderror-jpa-jpqlquery-from)

## 원인
원인은 생각보다 간단했습니다. 라이브러리에서 Querydsl 의 버전이 달라서 생긴 에러였습니다.

maven 에서 Querydsl 의 dependecy 를 걸을 때 평균적으론 이렇게 합니다. 사이트에서 이렇게 나오기때문이죠!
```xml
<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-jpa</artifactId>
    <version>4.3.1</version>
</dependency>

<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-apt</artifactId>
    <version>4.3.1</version>
</dependency>

<build>
    <plugins>
        <plugin>
            <groupId>com.mysema.maven</groupId>
            <artifactId>apt-maven-plugin</artifactId>
            <version>1.1.3</version>
            <executions>
                <execution>
                    <goals>
                        <goal>process</goal>
                    </goals>
                    <configuration>
                        <outputDirectory>target/generated-sources/java</outputDirectory>
                        <processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
    </build>
```

근데 이렇게 version 을 명시해주니 Querydsl 을 사용하는 사용자 입장에선 버전이 다르니 헷갈리기시작합니다. plugin의 Querydsl 1.1.3 버전에 맞춰서 dependency 의 querydsl 버전도 맞춰줘야하는데 둘의 버전을 맞춰주지못해서 에러가 발생했습니다.
그러면 어떻게 해야할까요????

## 해결방법
사실 뭐.. 해결방법이라고 할 것도 없습니다. plugin이 알아서 버전을 맞춰주게 해주는 것이죠
```xml
<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-jpa</artifactId>
</dependency>

<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-apt</artifactId>
</dependency>

<build>
    <plugins>
        <plugin>
            <groupId>com.mysema.maven</groupId>
            <artifactId>apt-maven-plugin</artifactId>
            <version>1.1.3</version>
            <executions>
                <execution>
                    <goals>
                        <goal>process</goal>
                    </goals>
                    <configuration>
                        <outputDirectory>target/generated-sources/java</outputDirectory>
                        <processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
    </build>
```

![스크린샷 2020-08-08 오후 6 38 29](https://user-images.githubusercontent.com/45488643/89707127-6d505080-d9a6-11ea-92a9-f8294821cd01.png)

비록 버전은 낮아졌지만 에러는 발생하지않았습니다 해결완료!  

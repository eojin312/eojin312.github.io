---
title: "generic-type"
date: 2020-01-21 22:42:28 -0400
categories: 개인공부
---


## 제네릭이란
제네릭은 **클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법을 의미** 합니다.
라고 하는데, 가장 와닿는 예제는 헤드퍼스트 자바에서 소개된 ArrayList 사례였습니다.

JDK5 이전까지는 제네릭이 없었고, 그래서 아래와 같은 사례로 컬렉션에서 하나씩 꺼내 쓸때마다 무조건 캐스팅을 해야만 했다고합니다.

```java
Fish fish = new Fish();
Dog dog = new Dog();
Dog dog2 = new Dog();
Cat cat = new Cat();

List animals = new ArrayList();

animals.add(fish);
animals.add(dog);
animals.add(dog2);
animals.add(cat);

for (int i = 0; i < animals.size(); i++ ) {
    if (animals.get(i) instanceof Fish) {
        Fish f = (Fish) animals.get(i);
    }
    
    if (animals.get(i) instanceof Dog) {
        Dog d = (Dog) animals.get(i);
    }
    
    if (animals.get(i) instanceof Cat) {
        Cat c = (Cat) animals.get(i);
    }
}
```
극단적인 케이스겠지만 컬렉션에 Object형으로 뭐든지 담을 수 있게 자유롭기에 누구든 실수로 약속안된 형태의 타입을 넣을 수 있습니다.
그래서, 컬렉션을 선언할 때 타입을 강제해서 정해진 타입만 넣을 수 있도록 제약하는게 제가 이해한 제네릭의 용도입니다.

```java
Fish fish = new Fish();
Dog dog = new Dog();
Dog dog2 = new Dog();
Cat cat = new Cat();

List<Fish> animals = new ArrayList<>();

animals.add(fish);
animals.add(dog); // Exception발생!!!!

```

아래는 헤드퍼스트의 내용을 캡쳐해두었습니다.
![KakaoTalk_Photo_2020-01-31-20-38-54](https://user-images.githubusercontent.com/45488643/73536690-d1c7ab80-4469-11ea-8289-eaf511142cd6.jpeg)

## 회고점
제네릭의 다양한 용도가 있는 것 같지만, 읽어도 읽어도 확실히 이해가 되지 않았습니다.
지금 제가 코드를 만들 때 컬렉션에서 제네릭을 사용하는 용도로 한정되어있지만, 
차근차근 제네릭의 다른 용도는 알아볼 계획입니다.
---
toc: true
toc_sticky: true
title: "HashMap에서 hash 값이 충돌한다면?"
date: 2018-08-08 22:00:00 +0900
categories: 
    - Java 
tags: 
    - HashMap
    - Map
    - Java
---

## Intro
Java의 `HashMap`은 `Map`의 구현체 중 하나로 이름 처럼 `Hash`를 `key`로 사용하는 것이 특징이다. `Hash`의 경우 그 특성항 객체의 실제 값이 다르더라도 같은 값을 반환할 수 있다. `HashMap`에서는 이 문제를 어떻게 해결하고 있는지 알아보자.

## Hash

### HashTable

Java에서는 `HashMap`이 나오기 전에 `HashTable` 이라는 클래스를 사용하고 있었다. 두 클래스 모두 `Map` 인터페이스를 구현한 구현체로 `Hash`를 `key`로 사용한다는 공통점이 있다. `HashTable`은 세상에 나온 이후로 변화가 거의 없는 반면 `HashMap`은 성능이나 `Hash collision`을 줄이기 위한 변화가 꾸준히 이루어지고 있다. 

### 그래서 Hash가 뭔데?

## 해쉬 충돌 (Hash collision)

## Outro


## References

- [http://ydtech.blogspot.com/2010/06/hashmap-hashcode-collision-by-example.html](http://ydtech.blogspot.com/2010/06/hashmap-hashcode-collision-by-example.html)
- [https://netjs.blogspot.com/2015/05/how-hashmap-internally-works-in-java.html](https://netjs.blogspot.com/2015/05/how-hashmap-internally-works-in-java.html)
- [https://d2.naver.com/helloworld/831311](https://d2.naver.com/helloworld/831311)
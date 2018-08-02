---
toc: true
toc_sticky: true
title: "Framework과 library 이야기"
date: 2018-08-02 21:00:00
categories: 
    - Terminology 
    - 용어
tags: 
    - Terminology
    - Framework
    - Library
---

## 들어가며

개발 업무를 하면서 참 많이 말하고 실제 사용하기도 하는 두 단어이다. 하지만 정확한 의미나 혹은 두가지 사이의 차이점이 무엇인지 설명하라고 하면 명쾌하게 설명할 자신은 없는것 같다. 그럼 공부해야 한다.
며
## 사전적 의미

짧은 영어 실력이지만 옥스포드 영어사전에서 검색하면 아래와 같은 사전적 정의를 찾아 볼 수 있다. **Library**의 일반적인 다른 의미는 언급하지 않고 *Computing*으로 표시된 부분만 살펴보겠다.

### Framework

건물, 차량 또는 사물의 지탱하는 기본 구조

체계(시스템), 개념 또는 글의 근본을 이루는 기본 구조

```
NOUN

1 An essential supporting structure of a building, vehicle, or object.
‘a conservatory in a delicate framework of iron’
    
    1.1 A basic structure underlying a system, concept, or text.
    ‘the theoretical framework of political sociology’

```

### Library

일반적으로 바로 사용할 수 있도록 디스크에 저장 및 로드되는 프로그램 및 소프트웨어 패키지의 모음.

```
NOUN

1.5 A collection of programs and software packages made generally available, often loaded and stored on disk for immediate use.
‘download any of thousands of programs from our software libraries’

```

## 그래서 뭐가 다르다고??

Framework와 library의 차이는 제어의 관점에서 보면 좀더 명확하게 보인다. 간단하게 정리해보자.

- 내가 호출 하는건 library
- 내가 호출 당하는건 framework

Library는 사전적 의미에서도 알 수 있듯이 재사용을 위해서 만들어 둔 것이다. 개발자는 코드 안에서 특정 기능을 수행하기 위해서 library를 호출하게 된다.

Framework는 이미 정의된 일정한 흐름이 존재한다. 개발자는 그 흐름에 맞추어 원하는 동작을 하도록 코드를 작성해 연동을 한다. Framework의 경우 일반적으로 복잡하고 그 자체만으로는 동작 하지 않으며 개발자의 코드가 있어야만 특정한 기능을 수행하게 된다. 이경우 framework에서 개발자의 코드를 호출하게 되며 이를 **제어의 역전**(**Inversion of Control**) 이라고 한다.

```
+---------+   You call Library   +-----------+
| Library | <------------------- | Your Code |
+---------+                      +-----------+
     A                                 A
     | contains                        |
     |                                 |
+-----------+    Framework call you    |
| Framework |--------------------------!
+-----------+

```

## References

- [Oxford Living Dictionaries](https://en.oxforddictionaries.com)
- [https://www.programcreek.com/2011/09/what-is-the-difference-between-a-java-library-and-a-framework/](https://www.programcreek.com/2011/09/what-is-the-difference-between-a-java-library-and-a-framework/)
- [https://medium.com/datafire-io/libraries-vs-frameworks-626cdde799a7](https://medium.com/datafire-io/libraries-vs-frameworks-626cdde799a7)
- [https://stackoverflow.com/questions/148747/what-is-the-difference-between-a-framework-and-a-library](https://stackoverflow.com/questions/148747/what-is-the-difference-between-a-framework-and-a-library)
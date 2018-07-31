---
title: "StringBuilder와 StringBuffer의 차이점"
date: 2018-07-31T05:21:23.000Z
categories: Java 자바
tag: StringBuilder StringBuffer Java 자바
---

# Do you know...?

어느 화창한 여름날 아무개가 나에게 물었다.

> 아무개: StringBuffer와 StringBuilder의 차이점을 아나요?

> 나: ... 생각해 본적이 없는거 같은데요.

Java에서 String과 String을 연결할때 가장 많이 사용하는 방법은 **+** 연산자를 사용한 방법이다. 예를 들면 아래와 같다.

```
"some string" + "another string"
```

하지만 많은 개발자들이 위의 방법은 성능적인 측면에서 좋은 방법이 아니라고 한다. 그리고 좀더 우아한 방법으로 **StringBuilder**와 **StringBuffer**를 사용하길 권장한다. 그래서 난 지금까지 아무 생각없이 이유도 없이 **StringBuilder**를 써왔다. 

다음에는 아무개의 질문에 답할 수 있도록 **StringBuilder**와 **StringBuffer**는 어떤 차이가 있고 정말 **+** 연산자를 사용하는건 안좋은지 확인해보자.

# StringBuilder

# StringBuffer

# 성능 비교하기

# 알고쓰자

# References

- https://docs.oracle.com/javase/tutorial/java/data/buffers.html
- https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuilder.html
- https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuffer.html
- http://www.baeldung.com/java-string-builder-string-buffer
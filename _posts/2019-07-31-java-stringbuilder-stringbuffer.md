---
classes: wide
title: "StringBuilder와 StringBuffer의 차이점"
date: 2018-07-31T05:21:23.000Z
categories: Java 자바
tags: StringBuilder StringBuffer Java
---

# Do you know...?

어느 화창한 여름날 아무개가 나에게 물었다.

> 아무개: StringBuffer와 StringBuilder의 차이점을 아나요?

> 나: ... 생각해 본적이 없는거 같은데요.

Java에서 String과 String을 합칠때 가장 많이 사용하는 방법은 **+** 연산자를 사용한 방법이다. 예를 들면 아래와 같다.

```
"some string" + "another string"
```

하지만 많은 개발자들이 위의 방법은 성능적인 측면에서 좋은 방법이 아니라고 한다. 그리고 좀 더 우아한 방법으로 **StringBuilder**와 **StringBuffer**를 사용하길 권장한다. 그래서 난 지금까지 아무 의심없이 **StringBuilder**를 써왔다. 위의 질문을 받기 전까지는...

다음에는 아무개의 질문에 답할 수 있도록 **StringBuilder**와 **StringBuffer**는 어떤 차이가 있고 정말 **+** 연산자를 사용하는건 안좋은지 확인해보자.

# StringBuffer

JDK1.0 부터 제공되던 class이다. JavaDoc을 살펴보면 두가지 설명이 눈에 띈다.

> A thread-safe, mutable sequence of characters. A string buffer is like a String, but can be modified.

> As of release JDK 5, this class has been supplemented with an equivalent class designed for use by a single thread, StringBuilder. The StringBuilder class should generally be used in preference to this one, as it supports all of the same operations but it is faster, as it performs no synchronization.

먼저 일반적인 String class가 immutable 해서 변경할 수 없다면, StringBuffer는 mutalbe 하기 때문에 그 내용을 수정할 수 있다. String class도 변경이 하능하다고 생각 할 수 있으나 새로운 String 객체를 만들어서 변수에 할당하고 이전 값은 GC가 동작할때 메모리에서 사라질 뿐이다.

또한 중요한 StringBuffer의 장점은 thread-safe 하다는 것이다. 하지만 동기화를 위해서 많은 자원을 소비하기 때문에 JDK1.5부터 StringBuilder라는 thread-safe하지 않은 class를 제공한다.

StringBuilder의 특징

- thread-safe
- mutable

# StringBuilder

StringBuffer에서 처럼 JavaDoc을 먼저 살펴보자.

> A mutable sequence of characters. This class provides an API compatible with StringBuffer, but with no guarantee of synchronization.

> Instances of StringBuilder are not safe for use by multiple threads. If such synchronization is required then it is recommended that StringBuffer be used.

변경이 가능한 일련의 문자들이며 StringBuffer와 호환되는 API를 제공한다. 하지만 동기화는 보장하지 않는다. 그리고 멀티스레드 환경에서 동기화가 필요한 경우에는 StringBuffer를 써라.

위의 StringBuffer에서 나왔던 것처럼 StringBuilder는 StringBuffer의 성능적인 문제를 해결하고자 thread-safe의 특성을 없애고 나온 class이다. 또한 StringBuffer와 호환이 되는 API를 제공한다. 

StringBuilder의 특징

- mutable
- performance

# 실제로 비교하기

Single thread 환경에서 String, StringBuffer, StringBuilder 사이의 단순 속도비교와 Multi thread 환경에서 StringBuffer, StringBuilder 의 thread-safe 여부를 확인해보자.

JavaStringCompare.java

```
import java.util.concurrent.atomic.AtomicBoolean;

public class JavaStringCompare {
    public String singleThreadStringConcat(int count_p) {
        String temp = "";
        for(int i=0; i < count_p; i++) {
            temp += "i";
        }
        return temp;
    }

    public String singleThreadStringBufferAppend(int count_p) {
        StringBuffer temp = new StringBuffer();
        for(int i=0; i < count_p; i++) {
            temp.append("i");
        }
        return temp.toString();
    }

    public String singleThreadStringBuilderAppend(int count_p) {
        StringBuilder temp = new StringBuilder();
        for(int i=0; i < count_p; i++) {
            temp.append("i");
        }
        return temp.toString();
    }

    public String multiThreadStringBufferAppend(int count_p) throws InterruptedException {
        StringBuffer temp = new StringBuffer();
        AtomicBoolean flagOne = new AtomicBoolean(false);
        AtomicBoolean flagTwo = new AtomicBoolean(false);

        new Thread(() -> {
            for(int i=0; i<count_p; i++) {
                temp.append("i");
            }
            flagOne.set(true);
        }).start();

        new Thread(() -> {
            for(int i=0; i<count_p; i++) {
                temp.append("i");
            }
            flagTwo.set(true);
        }).start();

        while(flagOne.get()==false && flagTwo.get()==false) {
            Thread.sleep(10L);
        }

        return temp.toString();
    }

    public String multiThreadStringBuilderAppend(int count_p) throws InterruptedException {
        StringBuilder temp = new StringBuilder();
        AtomicBoolean flagOne = new AtomicBoolean(false);
        AtomicBoolean flagTwo = new AtomicBoolean(false);

        new Thread(() -> {
            for(int i=0; i<count_p; i++) {
                temp.append("i");
            }
            flagOne.set(true);
        }).start();

        new Thread(() -> {
            for(int i=0; i<count_p; i++) {
                temp.append("i");
            }
            flagTwo.set(true);
        }).start();

        while(flagOne.get()==false && flagTwo.get()==false) {
            Thread.sleep(10L);
        }

        return temp.toString();
    }

}

```

JavaStringCompareTest.java

```
import org.testng.annotations.*;
import static org.testng.Assert.*;

public class JavaStringCompareTest {
    private JavaStringCompare stringCompare = new JavaStringCompare();
    private Integer count = 100000;

    @Test
    public void testSingleThreadStringConcat() {
        String s = stringCompare.singleThreadStringConcat(count);
        assertEquals(s.length(), count.intValue());
    }

    @Test
    public void testSingleThreadStringBufferAppend() {
        String s = stringCompare.singleThreadStringBufferAppend(count);
        assertEquals(s.length(), count.intValue());
    }

    @Test
    public void testSingleThreadStringBuilderAppend() {
        String s = stringCompare.singleThreadStringBuilderAppend(count);
        assertEquals(s.length(), count.intValue());
    }

    @Test
    public void testMultiThreadStringBufferAppend() throws InterruptedException {
        String s = stringCompare.multiThreadStringBufferAppend(count);
        assertEquals(s.length(), count.intValue()*2);
    }

    @Test
    public void testMultiThreadStringBuilderAppend() throws InterruptedException {
        String s = stringCompare.multiThreadStringBuilderAppend(count);
        assertEquals(s.length(), count.intValue()*2);
    }
}

```

Result

```
    JavaStringCompareTest.testMultiThreadStringBufferAppend
    129 ms passed
    
    JavaStringCompareTest.testMultiThreadStringBuilderAppend
    13 ms failed
        java.lang.AssertionError: expected [200000] but found [156642]
            Expected : 200000
            Actual : 156642
    
    JavaStringCompareTest.testSingleThreadStringBufferAppend
    6 ms passed
    
    JavaStringCompareTest.testSingleThreadStringBuilderAppend
    5 ms passed
    
    JavaStringCompareTest.testSingleThreadStringConcat
    7.37 s passed

```

테스트 결과는 충분히 예상이 가능했다. 

String의 **+** 연산의 경우 엄청난 차이가 발생했다. 다만 single thread 환경에서 StringBuffer의 성능이 유의미하게 나쁘게 생각되지 않는다. 물론 간단한 연산이어서 더욱 그럴 수 있다. 반복 횟수를 증가시킨 경우에는 2배이상 차이가 나기도 했다. 하지만 절대적인 성능 자체는 나쁘지 않았다.

여기서 중요하게 볼 부분은 테스트에 실패한 부분이다. Multi thread 환경에서 StringBuilder를 가지고 연산하는 경우 연산 결과가 기대한 값이 나오지 않음을 확인 할 수 있다. 이는 StringBuilder가 thead-safe하지 않기 때문이다.

# 결론

String 객체를 **+** 연산하는건 피하자.

StringBuilder를 사용하되 multi thread 환경이 예상되는 곳에서는 StringBuffer를 사용하자.

추가적으로 **+** 연산자를 사용하는 경우 Java compiler는 StringBuffer 혹은 이와 동등한 것으로 자동 치환한다고 한다. 단순히 치환만 하는것으로 보이며 반복문 안에 있는 경우에는 소용 없어 보인다.

> An implementation may choose to perform conversion and concatenation in one step to avoid creating and then discarding an intermediate String object. To increase the performance of repeated string concatenation, a Java compiler may use the StringBuffer class or a similar technique to reduce the number of intermediate String objects that are created by evaluation of an expression. 

# References

- https://docs.oracle.com/javase/tutorial/java/data/buffers.html
- https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuilder.html
- https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuffer.html
- http://www.baeldung.com/java-string-builder-string-buffer
- https://dzone.com/articles/string-concatenation-performacne-improvement-in-ja
- https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html#jls-15.18.1

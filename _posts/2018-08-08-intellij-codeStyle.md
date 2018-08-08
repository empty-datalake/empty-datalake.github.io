---
toc: true
toc_sticky: true
title: "Use code style scheme with IntelliJ"
date: 2018-08-08 17:00:00 +0900
categories: 
    - IntelliJ 
tags: 
    - IntelliJ
    - Code_style
    - Google_code_style
---

## Intro

여러명이 같이 개발을 하면서 코딩 스타일이 달라서 서로 불편함을 가지는 경우가 종종 발생한다. 불편하면 불편을 해소해야 한다. 

1. 코딩 스타일을 정하자.

2. 문서로 된 코딩 스타일을 안지키는 사람이 생긴다.

3. 아...

## IntelliJ can support...

IDE는 코딩 스타일에 대한 개발자의 고민을 덜어주기 위해서 정해진 스타일에 맞추어 코드를 정리해주는 기능을 제공한다. 당연히 ```IntelliJ``` 도 해당 기능이 있다.

- MENU > Code > Reformat Code (Ctrl+Alt+L)

위의 명령을 실행하면 지정된 코딩 스타일에 따라서 코드를 정리해준다. 그럼 실제 적용을 해보자. 

### Ugly Code

일부러 못생기게 만드는 코드가 더 어려운거 같다.

```java
public class Foo {

    public int[] X =
            new int[]
                    {1, 3, 5, 7, 9, 11}
            ;

    public void foo(
            boolean
                    a,
            int x,
            int y
            ,
            int z
    ) {
        label1:
        do{try {
         if (x > 0) {
                    int someVariable=a?x:y;
                        int anotherVariable=
                            a?x:y;
             } else
                 if(x<0) {
                    int someVariable = (y + z);
                    someVariable = x = x + y;
            } else {
            label2:
            for (int i = 0; i < 5; i++) {
            doSomething(i);
            }
            }
            switch (a) {
            case 0:
                doCase0();
            break;
            default:
                doDefault();
        }
            }
            catch
            (Exception e) {
                processException(
                        e.getMessage(), x + y, z, a
                );
            } finally {
                processFinally();
            }
        }








        while (true);

        if (2 < 3) {



            return;
        }
        if (3 < 4) {
            return;
        }
            do {
                x++;
            }
        while (x < 10000);
        while(x<50000) {x++;}
        for (int i = 0; i < 5; i++)
        {
            System.out.println(i);
        }
    }

    private class InnerClass
            implements I1, I2
    {

        public void bar()
                throws E1, E2
        {
        }
    }
}
```

### Reformat Code

IntellJ의 기본값을 이용해 수정한 코드이다.

- 들여쓰기는 tab이 아닌 space로 처리되며 4개의 space가 적용된다.
- 중괄호의 경우 호불호가 강한 부분인데 일단 줄바꿈을 하지 않고 괄호를 연다.
- 빈 줄바꿈 공간은 3줄 이상인 경우 2줄의 줄바꿈으로 변경된다.

```java
public class Foo {

    public int[] X =
            new int[]
                    {1, 3, 5, 7, 9, 11};

    public void foo(
            boolean
                    a,
            int x,
            int y
            ,
            int z
    ) {
        label1:
        do {
            try {
                if (x > 0) {
                    int someVariable = a ? x : y;
                    int anotherVariable =
                            a ? x : y;
                } else if (x < 0) {
                    int someVariable = (y + z);
                    someVariable = x = x + y;
                } else {
                    label2:
                    for (int i = 0; i < 5; i++) {
                        doSomething(i);
                    }
                }
                switch (a) {
                    case 0:
                        doCase0();
                        break;
                    default:
                        doDefault();
                }
            } catch
            (Exception e) {
                processException(
                        e.getMessage(), x + y, z, a
                );
            } finally {
                processFinally();
            }
        }


        while (true);

        if (2 < 3) {


            return;
        }
        if (3 < 4) {
            return;
        }
        do {
            x++;
        }
        while (x < 10000);
        while (x < 50000) {
            x++;
        }
        for (int i = 0; i < 5; i++) {
            System.out.println(i);
        }
    }

    private class InnerClass
            implements I1, I2 {

        public void bar()
                throws E1, E2 {
        }
    }
}
```

### Reformat Code with Google Style Guide

같이 일하는 개발자가 같은 스타일을 사용하기 위해서는 표준을 정해야 하는데 갑론을박이 많을 수 있다. 일단 구글님을 믿어보자.

```java
public class Foo {

  public int[] X =
      new int[]
          {1, 3, 5, 7, 9, 11};

  public void foo(
      boolean
          a,
      int x,
      int y
      ,
      int z
  ) {
    label1:
    do {
      try {
        if (x > 0) {
          int someVariable = a ? x : y;
          int anotherVariable =
              a ? x : y;
        } else if (x < 0) {
          int someVariable = (y + z);
          someVariable = x = x + y;
        } else {
          label2:
          for (int i = 0; i < 5; i++) {
            doSomething(i);
          }
        }
        switch (a) {
          case 0:
            doCase0();
            break;
          default:
            doDefault();
        }
      } catch
      (Exception e) {
        processException(
            e.getMessage(), x + y, z, a
        );
      } finally {
        processFinally();
      }
    }

    while (true);

    if (2 < 3) {

      return;
    }
    if (3 < 4) {
      return;
    }
    do {
      x++;
    }
    while (x < 10000);
    while (x < 50000) {
      x++;
    }
    for (int i = 0; i < 5; i++) {
      System.out.println(i);
    }
  }

  private class InnerClass
      implements I1, I2 {

    public void bar()
        throws E1, E2 {
    }
  }
}
```

위의 결과가 생각보다 변경점이 적어서 IntelliJ의 ```Keep when reformatting``` 옵션을 변경해 보았다.

```java
public class Foo {

  public int[] X = new int[]{1, 3, 5, 7, 9, 11};

  public void foo(boolean a, int x, int y, int z) {
    label1:
    do {
      try {
        if (x > 0) {
          int someVariable = a ? x : y;
          int anotherVariable = a ? x : y;
        } else if (x < 0) {
          int someVariable = (y + z);
          someVariable = x = x + y;
        } else {
          label2:
          for (int i = 0; i < 5; i++) {
            doSomething(i);
          }
        }
        switch (a) {
          case 0:
            doCase0();
            break;
          default:
            doDefault();
        }
      } catch (Exception e) {
        processException(e.getMessage(), x + y, z, a);
      } finally {
        processFinally();
      }
    }

    while (true);

    if (2 < 3) {

      return;
    }
    if (3 < 4) {
      return;
    }
    do {
      x++;
    } while (x < 10000);
    while (x < 50000) {
      x++;
    }
    for (int i = 0; i < 5; i++) {
      System.out.println(i);
    }
  }

  private class InnerClass implements I1, I2 {

    public void bar() throws E1, E2 {
    }
  }
}
```

## IntelliJ에 Google Code Style 적용하기

예시 처럼 google code style을 IntelliJ에서 사용 할 수 있다. 그러기 위해서는 먼저 style이 정의된 scheme을 받아야 한다.

- [IntelliJ jvav google style](https://github.com/google/styleguide/blob/gh-pages/intellij-java-google-style.xml) 

### 적용하기

- Settings > Editor > Code Style > Java 로 이동
- Scheme > 설정버튼 > Import Scheme > IntelliJ IDEA code style XML 선택
- 다운로드 받은 style xml 선택

## with Git

IntelliJ에서 Git을 사용중이라면 ```commit``` 하기전에 ```reformat code``` 를 할 수 있다.

- Commit > Commit Changes > Before Commit > Reformat code

```Reformat code``` 외에도 ```Rearrange code``` , ```Optimize import``` 등의 작업 수행이 가능하다.

## with Gradle

테스트 하지 않았지만 gradle에서 스타일 체크가 가능하다. 그럼 스타일 규칙에 어긋난 코드는 merge가 불가능하도록 할 수 있지 않을까?

- [Checkstyle Plugin](https://docs.gradle.org/current/userguide/checkstyle_plugin.html)

## Outro

거창하게 적었지만 IDE에서 지원하는 reformat 기능은 만능이 아니다. 물론 옵션에 따라서 획기적으로 코드를 정리할 수 있지만 오히려 가독성이나 편의성에 악영향을 줄 수 있다. 하지만 여러 개발자와 협업하는데 코딩 스타일에 문제가 있는 경우에 분명 효과적인 해결책이 될 수 있다.

## References

- [https://www.jetbrains.com/help/idea/2018.2/code-style-java.html?utm_content=2018.2&utm_medium=link&utm_source=product&utm_campaign=IU](https://www.jetbrains.com/help/idea/2018.2/code-style-java.html?utm_content=2018.2&utm_medium=link&utm_source=product&utm_campaign=IU)
- [https://github.com/google/styleguide/blob/gh-pages/intellij-java-google-style.xml](https://github.com/google/styleguide/blob/gh-pages/intellij-java-google-style.xml)

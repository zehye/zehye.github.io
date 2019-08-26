---
layout: post
title: Java - 람다식(lambda expressions)
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## 람다식 (Lambda Expressions)

익명함수 (anonymous function)을 이용해서 익명 객체를 생성하기 위한 식이다.

**즉, 인터페이스 타잉븨 변수를 두고 람다식을 통해 바로 인터페이스에 있는 메서드를 구현해 이용하는 것**

람다식은 기존 oop와는 달리 함수지향 프로그래밍 기법인데, 이는 곧 객체를 만들지 않고 함수로 이루어진 언어를 뜻한다. 객체지향 프로그래밍은 함수의 기능들을 묶어묶어 하나의 객체를 만드는 것인데, 간단한 함수로만 만들 수 있는 것들을 객체지향으로 하는 것에 대한 아쉬움을 해소하기 위해 함수만을 이용하는 방법을 생각하게 되었고 그로인해 만들어진 것이 람다식이다.

즉, 객체를 만들지 않고 메서드를 이용하고 싶을 경우, 메서드에 있어서 가장 핵심적인 부분(파라미터, 실행문 등)만을 만들어주고 바로 이용해 원하는 결과물을 빠르게 도출해낼 수 있다.

### 람다식의 구현

람다식은 기본적으로 `함수`를 만들어 사용한다고 생각하면 된다.

인터페이스를 이용해 함수의 껍데기를 만들고 그 함수를 인터페이스 데이터 타입 변수에 언제든지 담아서 우리가 구현하고 싶은데로 구현해 사용할 수 있다.

우선 각각의 여러 인터페이스들을 만들어보자.


#### LambdaInterface1, LambdaInterface2, LambdaInterface3, LambdaInterface4

```java
public interface LambdaInterface1 {
  public void method(String s1, String s2, String s3);
}
```

```java
public interface LambdaInterface2 {
  public void method(String s1);
}
```

```java
public interface LambdaInterface3 {
  public void method();
}
```

```java
public interface LambdaInterface4 {
  public int method(int x, int y);
}
```

#### MainClass

각각의 인터페이스들을 구현할 메인클래스를 생성

```java
public class MainClass {
  public static void main(String[] args) {

    // 매개변수와 실행문만으로 작성될 경우 접근자, 반환형, return 키워드 생략한다.
    LambdaInterface1 li1 = (String s1, String s2, String s3) -> { System.out.println(s1+""+s2+""s3); };
    li1.method("hello","java","world");

    // 매개변수가 1개이거나 타입이 같을 때, 타입을 생략할 수 있다.
    LambdaInterface2 li2 = (s1) -> { System.out.println(s1); };
    li2.method("Hello");

    // 실행문이 1개일떄 '{}'를 생략할 수 있다.
    LambdaInterface2 li3 = (s1) -> System.out.println(s1);
    li3.method("Hello");

    // 매개변수와 실행문이 1개일때, '()'와 '{}'를 생략할 수 있다.
    LambdaInterface2 li4 = s1 -> System.out.println(s1);
    li4.method("Hello");

    // 매개변수가 없을때 '()'만 작성한다.
    LambdaInterface3 li5 = () -> System.out.println("no parameter");
    li.method();

    // 반환값이 있는 경우
    LambdaInterface4 li6 = (x,y) -> {
      int result = x + y;
      return result;
    };
    System.out.printf("li6.method(10,20): %d\n", li6.method(10,20));

    // 반환값의 식을 변경하고 싶을때
    li6 = (x,y) -> {
      int result = x*y;
      return result;
    };
    System.out.printf("li6.method(10,20): %d\n", li6.method(10,20));
  }
}
```

#### console

```console
hello java world

Hello
Hello
Hello
no parameter
li6.method(10, 20) : 30
li6.method(10, 20) : 200
```

이렇듯 람다식은 요즘 유행하기 시작한 방식으로 이를 이용하면 코드가 유연해질 수 있기 때문에 개발자의 코딩량이 적게 들어갈 수 있다. 람다식을 모른다고 코딩을 못하는 건 아니지만(자바는 객체지향이므로) 우리가 협업을 할때 다른 사람들이 람다식을 많이 쓴다면 우리도 쓸줄 알아야하고, 볼줄도 알아야하니까 간단하게 숙지하고 있는 것은 중요할 듯 싶다.

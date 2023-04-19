---
layout: post
title: Java - 내부클래스와 익명클래스
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## 내부클래스

클래스안에 또 다른 클래스를 선언하는 방법으로 자주 쓰이는 방법은 아니다.

클래스 안에 또다른 클래스를 선언함으로써 두 클래스의 멤버에 쉽게 접근이 가능하지만 객체지향 프로그램은 클래스 단위로 객체를 뽑아내 객체들간의 관계를 이용해 프로그래밍화하는 것인데, 내부캘르스는 결국 객체안에 또 객체가 존재하게 하는 것으로 데이터를 쉽게 공유할 수 있는 것처럼 보이지만, 로직이 복잡해지기 시작하면서 데이터들이 꼬이게 될 수 있다.

#### OuterClass

```java
public class OuterClass(){
  int num = 10;
  String str1= "java";
  static String str11 = "world";
  public OuterClass(){
    System.out.println("OuterClass Constructor");
  }

  public class InnerClass(){
    int num = 100;
    String str2 = str1;

    public InnerClass(){
      System.out.println("InnerClass Constructor");
    }
  }

  static class SInnerClass(){
    int num = 1000;
    String str3 = str11;

    public SInnerClass(){
      System.out.println("Static InnerClass Constructor");
    }
  }
}
```

#### MainClass

```java
public class MainClass {
  public static void main(String[] args) {
    OuterClass oc = new OuterClass();
    System.out.println(oc.num);
    System.out.println(oc.str1);

    InnerClass ic = new InnerClass();
    System.out.println(ic.num);
    System.out.println(ic.str2);

    SInnerClass sic = new SInnerClass();
    System.out.println(sic.num);
    System.out.println(sic.str3);
  }
}
```

#### console

```console
OuterClass constructor
10
java

InnerClass constructor
100
java

Static InnerClass constructor
1000
world
```

## 익명클래스

이름이 없는 클래스로 주로 메서드를 재정의하는 목적으로 사용된다.

일반적으로 클래스를 통해 객체를 생성하기 위해서는

`AnonymousClass nc = new AnonymousClass();`를 통해 생성하는데 `new AnonymousClass();`라고도 표현함으로써 객체를 생성할 수가 있다. 이름이 없다는 것은 한번만 쓰고 버린다는 의미로 (이름이 없기때문에 생성된 객체를 다시 호출할 수 있는 방법이 없다) 단지 한번만 사용하고 버린다는 것을 뜻한다.

이러한 익명클래스는 재정의를 통해 한번 사용해서 쓰기도 하지만, 주로 인터페이스 혹은 추상클래스에 주로 사용된다.


### 익명클래스 사용방법

#### AnonymousClass

```java
public class AnonymousClass(){
  public AnonymousClass(){
    System.out.println("AnonymousClass Constructor");
  }

  public void method(){
    System.out.println("AnonymousClass method");
  }
}
```

#### MainClass

```java
new AnonymousClass() {

  @Override
  public void method(){
    System.out.println("AnonymousClass method() new start")
  };
}.method();
```

#### console

```console
AnonymousClass Constructor
AnonymousClass method() new start
```

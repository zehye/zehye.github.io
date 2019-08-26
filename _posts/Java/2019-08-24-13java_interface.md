---
layout: post
title: Java - 인터페이스
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## 인터페이스

클래스와 달리 객체를 생성할 수 없으며 클래스에서 구현해야하는 작업 명세서

일반적으로 클래스에서 new라는 키워드를 통해 객체를 메모리에 생성하고, 레퍼런스라는 변수를 이용해 속성과 기능을 사용했는데 인터페이스는 직접 객체를 생성할 수 없고 클래스에서 인터페이스를 구현하는 단계를 거친 뒤 인터페이스에서 선언만 되어있는 기능을 클래스에서 좀더 명확하게 정의하는 과정을 거쳐 객체를 생성한다.

(내용이 조금 어려우니 아래 예문으로 정리해보자)

### 인터페이스 사용이유

그전에 이러한 인터페이스를 사용하는 이유는

- 객체가 다양한 자료형을 가질 수 있기 때문이다.

>  인터페이스 A,B,C,D가 있다고 가정해보자.
이때 클래스는 인터페이스 A,B,C,D의 모든 기능들을 구현시킬 수 있으며 이는 곧 A,B,C,D에 대한 객체를 생성했다고 볼 수 있다.


#### InterfaceClass
```java
public class InterfaceClass implements InterfaceA, InterfaceB, InterfaceC, InterfaceD {
  public ImplementClass() {
    System.out.println("ImplementClass Constructor");
  }
}
```

#### MainClass

```java
InterfaceA ia = new ImplementClass();
InterfaceB ib = new ImplementClass();
InterfaceC ic = new ImplementClass();
InterfaceD id = new ImplementClass();
```

이때 각 객체들의 데이터타입은 InterfaceA, InterfaceB, InterfaceC, InterfaceD로 다양한 자료형을 가진다.


### 인터페이스 구현

class 대신 `interface` 키워드를 사용하며, extend 대신 `implements` 키워드를 사용한다.


#### InterfaceA

각각의 InterfaceA에 구현되는 함수는 정의없이 선언만 해준다.

그렇기 때문에 인터페이스를 작업명세서라고 이야기하는 것이고, 이렇게 선언만 되어있는 메서드는 정의부가 없기때문에 인터페이스 자체를 호출하는 것은 의미가 없는것이다. 각각의 인터페이스를 구현하는 것은 클래스에서 한다.


```java
public interface InterfaceA {
  public void funA(); // 추상메서드
}
```

#### InterfaceB

```java
public interface InterfaceB {
  public void funB();
}
```

#### InterfaceClass

만들어져 있는 두개의 인터페이스를 사용하기 위한 하나의 클래스 생성


```java
public class InterfaceClass implements InterfaceA, InterfaceB { // 상속과는 다르게 여러개 구현가능 -다형성
  public InterfaceClass() {
    System.out.println(" InterfaceClass constructor ");
  }

  @Override
  public void funA() {
    System.out.println(" funA() START ");
  }

  @Override
  public void funB() {
    System.out.println(" funB() START ");
  }
}
```

#### MainClass

InterfaceClass를 구현하기 위한 (객체 생성에 필요한) MainClass 생성


```java
public class MainClass {
  public static void main(String[] args) {
    InterfaceA ia = new InterfaceClass();
    InterfaceB ib = new InterfaceClass();

    ia.funA();
    ib.funB();
    // ia의 데이터타입은 InterfaceA이기 때문에 funA()에만 접근이 가능한 것
  }
}
```

즉, 인터페이스는 어떠한 기능은 없이 작업명세선만 있는 껍데기이며, 인터페이스에는 구체적인 기능이 정의되어 있지 않고 선언만 해놓는 정도로만 구현되어있다. 이때 인터페이스의 실질적인 구현은 이 인터페이스를 `implements`한 클래스에서 이루어진다.

implements한 클래스는 객체를 생성할 때 객체의 데이터 타입에 따라 사용할 수 있는 메서드가 있고 사용하지 못하는 메서드 또한 존재한다.

이 클래스로부터 만들어진 객체의 데이터타입은 각 인터페이스가 된다.

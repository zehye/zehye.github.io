---
layout: post
title: Java - 추상클래스(Abstract Class)
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## 추상클래스

- 추상적인 클래스, 구체화되지 않은 클래스
- 클래스의 공통된 부분을 뽑아서 별도의 클래스(추상 클래스)로 만들어놓고, 이것을 상속해 사용한다.
- 클래스와 인터페이스의 특징을 결합해 만든 클래스


### 추상클래스의 특징

- 멤버변수를 가짐
- abstract 클래스를 상속하기 위해서는 `extends`를 이용함
- abstract 메서드를 가지며, 상속한 클래스에서 반드시 구현해야 함
- 일반 메서드도 가질 수 있음
- 일반 클래스와 마찬가지고 생성자가 있음

즉, 클래스의 모든 기능을 가지면서도 인터페이스와 같이 선언만 되어있는 추상메서드도 가질 수 있다.


#### AbstractClassEx

```java
public abstract class AbstractClassEx {
  int num;
  String str;

  public AbstractClassEx() {
    System.out.println("AbstractClassEx constructor");
  }

  public AbstractClassEx(int i, String s) {
    System.out.println("AbstractClassEx constructor");

    this.num = i;
    this.str = s;
  }

  public void fun1() {
    System.out.println(" -- fun1() START -- ");
  }

  public abstract void fun2();
}
```

#### ClassEx

```java
public class ClassEx extends AbstractClassEx {
  public ClassEx() {
    System.out.println(" ClassEx constructor");
  }

  public ClassEx(int i, String s) {
    super(i, s);
  }

  @Override
  public void fun2(){
    System.out.println(" -- fun2() START -- ");
  }
}
```

#### MainClass

```java
AbstractClassEx ex = new ClassEx(10, "java");
ex.fun1();
ex.fun2();
```

#### console

```console
AbstractClassEx constructor
 -- fun1() START --
 -- fun2() START --
```

이렇게 보면 추상클래스나 인터페이스 둘 다 공통된 부분은 추상적으로 만들어놓고 프로그래밍을 할때 각자 알아서 해당 인터페이스와 추상 클래스를 `extends` 또는 `implements` 하여 알아서 구현할 수 있도록 해놓았다.

이렇게 알아서 구현을 한다는 것은 `다형성`을 만들 수 있다는 것을 의미한다. (다양한 객체 타입을 가진다.)


### 인터페이스 vs 추상클래스

#### 공통점

- 추상 메서드를 가진다.
- 객체 생성이 불가하며 자료형(타입)으로 사용된다.

#### 차이점

**인터페이스**

- 추상 메서드만 가진다.
- 추상메서드를 구현만 하도록 한다.
- 다형성을 지원한다.

**추상클래스**

- 클래스가 가지는 모든 속성과 기능을 가진다.
- 추상 메서드 구현 및 상속의 기능을 가진다.
- 단일 상속만 지원한다.

이렇듯 둘중 어느것 하나가 더 좋다기보다는 프로그램의 특징에 따라서
- 다양한 형태를 추구하는 프로그램에는 인터페이스
- 단일 상속을 지원하면서 공통된 부분의 메서드를 뽑아내 각각의 구현을 하게만들때는 추상클래스

즉, 설계에 따라 선택하면 된다.

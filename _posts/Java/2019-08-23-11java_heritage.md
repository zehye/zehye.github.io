---
layout: post
title: Java - 상속 (상속의 필요성과 상속 구현방법)
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## 상속

객체지향 프로그래밍에서 상속은 다른 객체(상위클래스)의 특정한 속성과 기능을 물려받아 내가 구현하지 않았어도 해당 속성과 기능을 내것처럼 사용할 수 있는 것

즉, 기존에 만들어진 클래스의 속성과 기능을 상속받아 새로운 클래스를 쉽게 만들 수 있다.

상위 클래스의 속성과 기능을 내가 만들지 않았음에도 상위클래스에서 생성된 객체를 똑같이 사용 가능함으로써 확장된 객체가 만들어질 수 있다.

### 상속의 필요성

기존의 검증된 클래스를 이용함으로써 빠르고 쉽게 새로운 클래스를 만들수 있다.

클래스를 만들때 물론 처음부터 끝까지 스스로 전부 구현해낼 수도 있지만, 클래스가 복잡해질수록 처음부터 끝까지 만들기보다는 비슷한 클래스가 있다면 상속을 받아 내가 지금 필요한 몇가지만 추가해 쉽고 빠르게 만드는 것이 가능해진다. 그리고 이렇게 상속을 하게되면 기존 클래스는 이미 검증되어있는 클래스이기때문에 재사용할 경우 더욱 견고해져있을 것이다. (`검증된 클래스`)

### 상속 구현 - extends

- extends키워드를 통해 상속 구현
- ChildClass가 생성되는 순간 ChildClass의 객체보다 상위클래스 (ParentClass)의 객체가 먼저 생성됨
  - 상위 클래스가 먼저 메모리에 객체를 만들어주어야 하위클래스의 메모리또한 생성이 됨
  - 즉, 상속 관계의 최상위 클래스를 먼저 생성해야 그 다음 클래스 생성이 가능해짐

- 자바 언어에서 다중상속은 지원하지 않음 > 오로지 `단일상속`
  - 즉, extends ~class, ~class, ~class 이러한 구조가 존재하지 않는다는 의미!
  - 하나의 클래스는 하나의 클래스만을 상속받을 수 있다.


#### ParentClass

```java
public class ParentClass {
  public ParentClass() {
    System.out.pringln("--ParentClass Constructor--");
  }
  public void parentFun(){
    System.out.println("--parentFun()--");
  }
}
```

#### ChildClass

```java
public class ChildClass extends ParentClass(){
  public ChildClass(){
    System.out.println("--ChildClass--");
  }

  public void childFun(){
    System.out.println("--childFun()--");
  }
}
```

#### MainClass

```java
ChildClass child = new ChildClass();

child.parentFun();
child.childFun();
```

#### console

```console
--ParentClass--
--ChildClass--
--parentFun()--
--childFun()--
```

### 부모클래스의 Private 접근자

자식 클래스는 부모 클래스의 모든 지원을 사용할 수 있지만, 부모 클래스에 `private`접근자의 속성과 메서드는 사용이 불가능하다.

**즉, 부모클래스의 `private`접근자의 속성과 메서드는 자식클래스에서 사용 불가능, 상속에서 제외됨!**

<hr>

추가로, 우리가 프로그래밍 언어에서 서로간의 관계를 나타내기 위해 다이어그램을 많이 사용하는데 상속에는 화살표가 사용된다.

화살표의 방향은 상속을 해주는 상위클래스 쪽으로 실선 화살표를 그려주고 있으며, 이는 상속관계를 의미한다.

<hr>

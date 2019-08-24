---
layout: post
title: Java - 상속의 특징(Override, datatype, ObjectClass, super)
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

앞서, [Java - 상속 (상속의 필요성과 상속 구현방법)](https://zehye.github.io/java/2019/08/23/11java_heritage/) 에 이어 상속의 큰 특징들에 대해 정리해봅니당


### 메서드 오버라이드 (Override)

부모클래스의 기능을 자식 클래스에서 재 정의해서 사용하는 방법

상위클래스의 기능을 엊데이트하여 더 좋은 기능으로 만드는 방법으로 이렇게 생성한 객체는 상위클래스, 하위클래스 객체를 모두 가진다. (기존의 상위클래스의 객체, 하위클래스에서 재정의하여 만든 객체 모두 가진다는 뜻)


#### ParentClass

```java
public class ParentClass(){
  public void parentFun(){
    System.out.println("--parentFun()--");
  }
}
```

#### ChildClass

```java
public class ChildClass extends ParentClass(){

  @Override
  public void parentFun(){
    System.out.println("--no! new function--");
  }
}
```

#### MainClass

```java
ChildClass child = new ChildClass();

child.parentFun();
```

#### console

```console
--no! new function--
```


### 자료형(타입)

기본 자료형들과 마찬가지로 클래스도 그 자체로 자료형이다.

`ChildClass child = new ChildClass();`라고 할때 child의 데이터 타입은 `ChildClass`이다.


### ObjectClass

모든 클래스의 최상위 클래스는 `ObjectClass`이다.

최상위 클래스가 ParentClass 같아 보이겠지만, 사실 가장 최상위에 있는 클래스는 ObjectClass이다.


### super 클래스

상위 클래스를 호출할 때 `super`키워드를 이용한다.


#### ParentClass

```java
int yearNum = 2018;

public void yearNum(){
  System.out.println("-- yearNum() --");
}
```

#### ChildClass

```java
public ChildClass extends ParentClass(){
  int yearNum = 2020;

  public void getYearNum(){
    System.out.println(" get nextYear: " + this.yearNum);
    System.out.println(" get lastYear:" + super.yearNum);
  }
}
```

#### MainClass

```java
ChildClass child = new ChildClass();
child.getYearNum();
```

#### console

```console
get nextYear: 2020
get lastYear: 2018
```

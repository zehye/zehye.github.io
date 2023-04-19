---
layout: post
title: Java - 장난감 인터페이스(예시)
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

인터페이스에 대한 이해를 돕기위해 흔히 예시를 삼는 장난감 인터페이스에 대해 정리해 봅니당.

### Toy

장난감 Toy 인터페이스를 구현헤본다.

이 인터페이스는 walk, run, alarm, light 추상메서드만 존재한다.

```java
public interface Toy {

  public void walk();
  public void run();
  public void alarm();
  public void light();
}
```

#### ToyRobotClass

인터페이스 Toy의 각 기능들을 구현할 ToyRobot 클래스를 생성하며 인터페이스에 있는 함수들의 구체적인 작업을 만들어준다.


```java
public class ToyRobotClass implements Toy {

  @Override
  public void walk() {
    System.out.println(" ToyRobot walk ");
  }

  @Override
  public void run() {
    System.out.println(" ToyRobot run ");
  }

  @Override
  public void alarm() {
    System.out.println(" ToyRobot alarm ");
  }

  @Override
  public void light() {
    System.out.println(" ToyRobot light ");
  }
}
```

#### ToyAirplaneClass

인터페이스 Toy의 각 기능들을 구현할 ToyAirplane 클래스를 생성하며 인터페이스에 있는 함수들의 구체적인 작업을 만들어준다.


```java
public ToyAirplaneClass implements Toy {

  @Override
  public void walk() {
    System.out.println(" ToyAirplane walk ");
  }

  @Override
  public void run() {
    System.out.println(" ToyAirplane run ");
  }

  @Override
  public void alarm() {
    System.out.println(" ToyAirplane alarm ");
  }

  @Override
  public void light() {
    System.out.println(" ToyAirplane light ");
  }
}
```

#### MainClass

```java
public class MainClass {
  public static void main(String[] args){
    Toy robot = new ToyRobotClass();
    Toy airplane = new ToyAirplaneClass();

    Toy toys[] = {robot, airplane};

    for (int i=0; i<toys.length; i++;){
      toys[i].walk();
      toys[i].run();
      toys[i].alarm();
      toys[i].light();
    }
  }
}
```

### 인터페이스의 장점

이렇듯 인터페이스를 이용하면 객체가 확장될 수 있다.

각 객체의 데이터 타입이 `다형성`을 통해 다양한 객체 타입을 가질 수 있으며, 인터페이스에서 선언만 해주고 실제 구현은 인터페이스를 구현한 객체에서 하기 때문에 코딩을 하면서 그때그때 수시로 처음부터 객체를 새로 만들것이 아니라 인터페이스에서 제공해주는 메서드에 따라 객체의 성격에 맞게 개성있게 기능을 구현할 수 있다.

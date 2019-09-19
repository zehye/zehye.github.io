---
layout: post
title: Java - 예외처리(Exception)
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## 예외란?

프로그램에 문제가 있는 것을 말하며, 예외로 인해 시스템 동작이 멈추는 것을 막는 것을 `예외처리`라고 한다.

개발자의 입장에서 프로그램이 실행이 되다가 어떠한 예외상황으로 인해 시스템이 중단된다면 무척 큰 어려움을 닥치게 될 것이다. 따라서 시스템 흐름에는 방해가 되지 않게 문제를 해결할 수 있는 코드를 생각/예측하여 처리를 해주어야 한다.

즉, 프로그램에 문제가 있을 때 프로그램이 중단될 수 있는데 그 예외를 미치 예측하여 전체적인 시스템 작동에는 문제가 없도록 처리는 것을 예외처리라고 한다.


### Exception / Error ?

`예외(Exception)`은 소프트웨어적으로 구현이 잘못된것을 의미한다. 즉, 개발자가 어떠한 연산을 잘못넣었다든지와 같은 구현에서의 잘못된 상황을 의미하는 것으로 이는 개발자가 직접 대처가 가능하다.

- CheckedException: 예외처리를 반드시 해야하는 것. 즉, 어떤 구문을 작성해야할 때 반드시 예외처리를 만들어야하며 만들지 않을 경우 컴파일 조차 작동되지 않으며 에러를 뱉어낸다.
  - 파일시스템, 입출력, 데이터 네트워크 등을 코딩할 때 자바 컴파일러가 예외처리를 하도록 강제적으로 제기
  - 예외처리를 하지 않는다면 컴파일 조차 작동하지 않음
<br>
- UnCheckedException: 개발자의 역량으로 예외처리를 하는 것. 기본적으로 예외처리를 하지 않아도 되지만, 개발자가 혹시모를 상황에 대비하여 예외처리를 하는 경우이다.
  - 데이터 오류 등

`에러(Error)`는 물리적인 장애요소(하드웨어)가 생긴것으로 개발자가 대처를 할 수 없다. 물리적인 장애요소란 예로들어 메모리부족, JVM 문제, 파워 오프 등 소프트웨어적으로 해결이 불가능한 것을 의미한다.  


### Exception 클래스

Exception 클래스의 하위클래스에는 `NullPointerException`, `ArrayIndexOutOfBoundsException`, `NumberFormatException`등이 있다.

- NullPointerException : 객체를 가리키지 않고 있는 레퍼런스를 이용할때 발생하는 예외
- ArrayIndexOutOfBoundsException : 배열의 크기를 벗어난 인덱스를 가리킬때 발생하는 예외
- NumberFormatException : 숫자가 아닌 문자가 들어와 연산이 불가능할때 발생하는 예외

이 외에도 굉장히 많은 Exception이 존재하며 이는 다 외울 필요도 없으며 그냥 이러한 애들이 있다는 정도로만 이해하고 넘어가면 된다.


### try~catch문

개발자가 예외처리하기 가장 쉽고, 많이 사용하는 방법이다.

#### MainClass

```java
public class MainClass {
  public static void main(String[] args){
    int i = 10;
    int j = 0;
    int r = 0;

    try {
      r = i / j;
    } catch (Exception e) {
      e.printStackTrace(); // 에러에서 어떤 예외가 발생했는지 콘솔창에 출력
      String msg = e.getMessage(); // 예외를 간략하게 나타낸 문자열
      System.out.println(msg);
    }
  }
}
```

#### console

```console
Exception: / by zero
java.lang.ArithmeticException: / by zero
	at (경로)
```



### 다양한 예외처리

Exception 및 하위 클래스를 이용해 다양한 예외처리를 할 수 있다.

#### MainClass

```java
import java.util.ArrayList;
public class MainClass{
  public static void main(String[] args){

    Scanner scanner = new Scanner(System.in); // 나중에 사용자로부터 값을 받을 것
    int i,j;
    ArrayList<String> list = null;

    int[] iArr = {0,1,2,3,4};

    try {
      System.out.println("int i:");
      i = scanner.nextInt();
      System.out.println("int j");
      j = scanner.nextInt();

      System.out.println("i / j = "+ i/j);

      for (int k = 0; k<6; k++) {
        System.out.println(iArr[k]);
      }

      System.out.println(list.size()); // 에러 발생지점
    } catch (InputMismatchException e) {
      e.printStackTrace();
    } catch (ArrayIndexOutOfBoundsException e) {
      e.printStackTrace();
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}
```

Exception 상위 클래스 하나로 예외를 받아도 되지만, 다양한 Exception들로 예외를 받아 처리할수도 있다.

그리고 try구문내에서 예외가 발생할 경우 그 아래 구문들은 실행이 되지 않는다.

#### console

```console
input i :
10
input j :
5
i / j = 2
iArr[0] : 0
iArr[1] : 1
iArr[2] : 2
iArr[3] : 3
iArr[4] : 4

java.lang.ArrayIndexOutOfBoundsException: 5
	at (경로)
```
해당 에러가 발생하는 이유는 인덱스의 크기는 5인데, 6을 가리키려고 하니까 발생하는 것!


### finally

예외 발생여부와 상관없이 반드시 실행된다.

일반적으로 try 구문내에서 에러가 하나라도 발생이 되면 그 아래 구문들은 실행이 되지 않고 바로 catch로 넘어가게 된다. 그러나 어떠한 예외가 발생해도 반드시 실행되어 넘어가져야하는 애들이 있는데 (일반적으로 네트워크 상에서 자원을 끌어쓸때를 말한다. - 데이터베이스) 이때 예외가 발생했다고 하더라도 반드시 네트워크를 끊는 작업을 해줘야 한다.

- try: 예외가 발생할만한 코드를 집어넣는다.
- catch: try내에서 발생한 예외는 다양한 Exception으로 구분하여 확인가능하다.
- finally: 예외가 발생하든 안하든 반드시 실행되어지는 구문이다.

```java
try {
  ...
} catch (Exception e) {
  ...
} finally {
  System.out.println("예외 발생 여부에 상관없이 언제나 실행");
}
```

### throws

예외 발생 시 예외 처리를 직접하지 않고 호출한 곳으로 넘긴다.

지금까지의 예외처리는 내가 알아서, 즉 전체시스템에 예외가 발생하지 않도록 직접 구문을 작성해서 만들어놓았는데 throws는 반대로 내가 실행하다가 예외가 발생하면 나를 호출했던 곳으로 다시 돌려보내는것을 의미한다.

**예외가 발생했을 때 내가 직접 해결하지 않고 나를 호출한 곳에서 해결하는 것**

#### MainClass

```java
public class MainClass {
  public static void main(String[] args){
    MainClass mainClass = new MainClass();

    try {
      mainClass.firstMethod(); // (4) 메인클래스로 와서 결국 예외를 잡는다
    } catch (Exception e){
      e.printStackTrace();
    }
  }

  public void firstMethod() throws Exception {
    secondMethod(); // (3) firstMethod 호출한 곳으로 이동
  }
  public void secondMethod() throws Exception {
    thirdMethod(); //(2) secondMethod 호출한 곳으로 이동
  }
  public void thirdMethod() throws Exception {
    System.out.println(10/0); // (1) 에러 발생 따라서 thirdMethod 호출한 곳으로 이동
  }
}
```

즉, 예외처리에 있어서는 2가지만 기억하면 된다.

**내가 직접 처리하는 것과 나를 호출하는 곳으로 돌려보내는 것**

예외는 실무에서 작업하다보면 무수히 많은 예외들을 접하게 되고, 이 예외들을 내가 설계 과정에서 판단 후 각 상황에 맞도록 구현을 하면 된다. (내가 처리할지, 돌려보낼지)

---
layout: post
title: Java - 생성자, 소멸자, this 키워드
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

### 디폴트 생성자

객체가 생성될 떄 가장 먼저 호출되는 생성자로 만약 개발자가 명시하지 않아도 컴파일 시점에서 자동 생성된다.

```java
ObjectEx obj1 = new ObjectEx();
```
- ObjectEx클래스를 통해 ObjectEx라는 타입의 obj1이라는 레퍼런스를 생성
- 메모리에는 ObjectEx라는 객체가 생성됐을 것이고, 해당 객체는 obj1가 레퍼런스 하고있음

- new라는 키워드를 통해 객체가 생성될 시점에 default 생성자가 호출
- 만약 디폴트생성자가 없는 경우 컴파일 시점에 컴파일러가 자동으로 알아서 디폴트 생성자를 생성
  - 생성자가 있어야 new 키워드를 통해 객체를 생성할 수 있음
- 따라서 객체는 정상적으로 생성이 됨


### 사용자 정의 생성자

디폴트 생성자와 다르게 특정 목적에 의해 개발자가 만든 생성자로 매개변수의 차이가 있다.

**파라미터를 던져 사용자 정의 생성자를 만들 수 있으며, 기본자료형/배열 등 모두 가능하다.**

#### ObjectClass

```java
public class ObjectClass {
  public ObjectClass(int[] iArr, String s) {
    System.out.println(s);
    System.out.println(iArr);
  }
}
````

#### mainClass

```java
public class mainClass {
  public static void main(String[] args){
    int iArr = {10,20,30};
    ObjectClass obj1 = new ObjectClass(iArr, "Hello");
  }
}
```


### 소멸자

객체가 GC에 의해 메모리에서 제거될 때 finalize() 메서드가 호출된다.

```java
ObjectEx obj4; // ObjectEx 객체와 obj4 레퍼런스
obj4 = new ObjectEx(); // ObjectEx()클래스에 obj4레퍼런스 생성
obj4 = new ObjectEx(); // ObjectEx()클래스에 obj4레퍼런스 재 생성, 따라서 이전 관계는 끊어짐

system.gc(); // gc 작동해주세요
```
```java
@Override
	protected void finalize() throws Throwable {

		System.out.println(" -- finalize() method --");

		super.finalize();
  }
```
기본적으로 `finalize()` 혹은 `system.gc`는 잘 사용하지 않는다.

자바에서 메모리는 개발자가 직접 관리하지 않으며 뿐만 아니라, `system.gc`를 사용한다고해서 GC가 바로 작동하는 것도 아니다. (단순히 가급적 빨리 작동하도록 요청하는 것일 뿐) 따라서 이 명령어를 직접사용하는 경우는 매우 드물다.


### this 키워드

현재 내가 작업하고 있는 해당 객체를 가리킨다.


#### ObjectClass2

```java
public class ObjectClass2 {
  public int x; // 전역변수
  public int y;

  public ObjectClass2(int x, int y){ // 지역변수
    this.x = x;
    this.y = y;
  }

  public void getInfo(){
    System.out.println(x);
    System.out.println(y);
  }
}
```

- 전역변수: 객체가 생성되고 소멸될 때까지 메모리에 항상 존재하는 데이터/변수
- 지역변수: 그 순간 활용되는 데이터로 활용된 이후 메모리에서 사라지는 데이터


#### MainClass2

```java
public class MainClass2{
  ObjectClass2 obj2 = new ObjectClass2(10,20);
}
```

- 즉, `public ObjectClass2(int x, int y)`에서 x,y가 생성이 될때
- MainClass2(호출부)에서 (10,20)이라는 매개변수를 던지면
- 그 던져진 (10,20)이 잠깐 담겨있다가 메모리에서 바로 사라진다.


위 예문에서 봤던 아래는 결국

```java
this.x = x;
this.y = y;
```

ObjectClass2에서 만들어지는 해당 객체를 가리키는 것으로 나 자신의 객체를 의미하며,

따라서 `이 객체전체에 속해있는 객체중에 존재하는 x` = `생성자로 전달된 파라미터 x`를 뜻한다.

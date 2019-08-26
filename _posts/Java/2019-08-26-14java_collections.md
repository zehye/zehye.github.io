---
layout: post
title: Java - Collections (List, Map)
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## Collections

프로그래밍을 하다보면 프로그램은 결과적으로 데이터를 주거니 받거니 하는 과정을 많이 거치게 되는데, 그럴때 데이터를 단순하게 하나씩 하나씩 주고받는 경우도 있지만 복잡한 데이터를 다룰 경우 클래스를 이용하게 된다. 이렇게 자바에서 데이터를 제공하고 관리해주는 클래스를 관리하는 방법에 대해 알아보자.

### List

- List는 인터페이스로 이를 구현한 클래스는 인덱스를 이용해 데이터를 관리
- 배열과 동일(배열은 복수의 데이터를 인덱스를 텅헤 데이터를 관리하는 것)
  - 데이터 중복이 가능

List라는 인터페이스를 구현하기 위한 클래스를 생성하고 그중에서도 우리는 `ArrayList`를 많이 사용한다.


#### MainClass

```java
import java.util.ArrayList;
public class MainClass {
  public static void main(String[] args) {

    // ArrayList 객체 생성
    // ArrayList라는 객체 타입을 만들어주고 ArrayList로 관리할 데이터 타입을 만들어준다
    ArrayList<String> list = new ArrayList<string>(); // 즉, <String>객체로 채워질 것을 명시
    // 데이터 개수
    System.out.println(list.size()); // 0

    // 데이터 추가
    list.add("hello");
    list.add("java");
    list.add("world");

    System.out.println(list.size); // 3
    System.out.println(list); // [hello, java, world]

    // 해당 인덱스에 데이터 추가
    list.add(2, "programming");
    System.out.println(list); //[hello, java, programming, world]

    // 해당 인덱스의 데이터 변경
    list.set(1, "C");
    System.out.println(list); // [hello, C, programming, world]

    // 데이터 추출
    String str = list.get(2);
    System.out.println(str); // programming
    System.out.println(list); //[hello, C, programming, world]

    // 데이터 제거
    str = list.remove(2);
    System.out.println(str); // programming
    System.out.println(list); // [hello, C, world]

    // 데이터 전체 제거
    list.clear();
    System.out.println(list); // []

    // 데이터의 유무
    boolean b = list.isEmpty();
    System.out.println(b); //true
  }
}
```



### Map

- Map 은 인터페이스로 이를 구현한 클래스는 `key`를 이용해 데이터를 관리한다.
- 반드시 `key`값이 있어야 하며 이 key값과 매칭되는 `value`값도 있어야 한다.
- 데이터 중복이 가능하지만, **key 값은 중복되면 안된다.**

Map 구현을 위해서는 `HashMap`이 쓰인다.


#### MainClass

```java
public class MainClass {
  public static void main(String[] args) {

    // HashMap 객체 생성(key, value, 사용할 데이터)
    HashMap<Integer, String> map = new HashMap<Integer, String>();
    System.out.println(map.size); // 0

    // 데이터 추가
    map.put(5, "hello");
    map.put(6, "java");
    map.put(7, "world");
    System.out.println(map); // {5=hello, 6=java, 7=world}
    System.out.println(map.size); // 3

    map.put(8, "!!");
    System.out.println(map); // {5=hello, 6=java, 7=world, 8=!!}

    // 데이터 교체
    map.put(6, "C");
    System.out.println(map); //{5=hello, 6=C, 7=world, 8=!!}

    // 데이터 추출
    str = map.get(5);
    System.out.println(str); // hello

    // 해당 키값의 데이터 삭제
    map.remove(8);
    System.out.println(map); // {5=hello, 6=java, 7=world}

    // 해당 key값에 데이터가 있는지?
    b = map.containKey(7);
    System.out.println(b); // true

    // 해당 value가 있는지?
    b = map.containValue("world");
    System.out.println(b); // true

    // 데이터 전체 삭제
    map.clear();
    System.out.println(map); // {}

    // 데이터의 유무
    b = map.isEmpty();
    System.out.println(b); // true
  }
}
```

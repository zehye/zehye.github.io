---
layout: post
title: Java - 문자열 클래스(StringBuffer, StringBuilder)
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## String 객체와 메모리

문자열을 다루는 String클래스(객체)는 데이터가 변하면 메모리상의 변화가 많아 속도가 느려진다.

스트링 객체의 약간의 단점을 꼽자면,

- String str = "JAVA";  메모리에 새로운 객체를 생성한 뒤
- str = str + "-8";  새로운 스트링 추가

이때 기존 JAVA 객체를 재활용하는게 아니라 객체를 복사해 새로운 메모리 공간에 새로운 객체를 생성한다. 이렇게 새로운 객체가 생성이 되면 기존의 객체는 GC에 의해 회수가 이루어진다.

기존 객체를 복사 후 새로운 메모리 공간을 차지하게 됨으로써 새로운 글자를 추가했을 뿐인데, 메모리상에서는 두개의 공간을 사용하게 되는 것이다. (메모리 효율이 떨어지는 단점 존재) 단지 기존 스트링에 약간의 스트링을 추가했을 뿐인데, 해당 객체를 가리키는 주소도 달라지게 된다.

기존 객체는 더이상 레퍼런스가 없어지게 되면서 GC에 의해 회수가 이루어지지만 회수를 하기 전까지도 메모리에는 계속 잡혀있기 때문에 메모리상에서의 효율은 (약간)떨어지게 되고 아주 미세하게 하드웨어의 속도도 저하될 수 있다.

### StringBuffer, StringBuilder

String 클래스의 단점을 보완한 클래스로 데이터가 변경되면 메모리에서 기존 객체를 재활용한다.


#### MainClass / StringBuffer

```java
public class MainClass {
  public static void main(String[] args) {
    StringBuffer sf = new StringBuffer("JAVA"); // 객체생성
    System.out.println(sf);

    sf.append(" world"); // 추가
    System.out.println(sf);

    System.out.println(sf.length()); // 글자 수

    sf.insert(sf.length(), "~~"); // 마지막에 해당 글귀 추가 -> insert(몇번째에 ~추가)
    System.out.println(sf)  

    sf.delete(4,8); // (x,y) -> x부터 y까지 삭제
    System.out.println(sf);
  }
}
```


#### MainClass / StringBuilder

```java
StringBuilder sb = new StringBuilder("JAVA WORLD!!");
System.out.println(sb);
```

이렇듯 단순히 String은 새로운 객체를 추가할 경우 새로운 메모리 공간에 객체를 복사해 추가를 했는데(그렇기 때문에 서로의 메모리 주소가 각기 다른데), StringBuffer와 StringBuilder는 `기존 메모리 공간에 추가하는 방식`을 사용하기 때문에 속도 저하도 없고 메모리를 비효율적으로 사용하지도 않는다.


### StringBuffer와 StringBuilder의 특징

기본적으로 StringBuffer는 데이터의 안정성이 좋고 StringBuilder는 빠르다고 한다.

사실 그 차이는 크지 않지만, 저렇게 설명하는 이유는 StringBuffer가 StringBuilder보다 먼저 나왔기 때문이다. 스트링의 단점을 보완하기 위해 StringBuffer가 나왔고 속도를 개선하기 위해 만들어진 것이 StringBuilder이다.

데이터의 안정성이 좋다는 의미는 메모리에 데이터가 들어가고 빠질때 `Synchronezed`기법을 사용하는데 이 기법은 메모리가 순차적으로 들어올때 하나씩 하나씩 줄을 세워 나열하는 방식을 말한다. (즉 순차적으로 메모리를 받는것) 그렇기 때문에 속도는 약간 늦어져도 데이터가 누실되거나 훼손되는 경우가 거의 없다.

반면 StringBuilder는 Synchronezed 기법이 아닌 들어오는 대로 받는 방식인데, 그렇기 때문에 안정성은 조금 낮아도 속도는 더 좋다.

그런데 이러한 특징은 사실 메모리 관점에서 아주 극한적인 상황일때 알수 있는 것으로 사실 둘중 아무거나 사용해도 상관은 없다.(다만 요즘 추세는 `StringBuilder` - 굉장히 많은 데이터를 다룰때는 속도가 좀더 중요한 이슈이기에..)

사실 근데 더 큰 관점에서 바라보자면, String이라는 객체는 문자열을 쉽게 다룰 수 있는데, 스트링의 단점이 있다고 해서 문제가 될 만큼 속도가 저하되거나 그런건 없기때문에 일반적으로 StringBuffer와 StringBuilder도 거의 사용하지 않는다.

우리가 데이터를 장기적으로 다루고 변하고 누적된다면 그때 사용하는 것이 StringBuffer, StringBuilder이고 수시로 데이터가 바뀌고 잠시동안만 사용한다면 그냥 String을 써도 전혀 우리는 차이를 느끼지 못할 것이다.

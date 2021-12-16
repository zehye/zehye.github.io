---
layout: post
title: swift - Equatable
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Equatable

Equatable은 프로토콜이다.

즉, Equatable이라는 약속이 있다는 것을 의미하며, 이를 채택하면 이를 준수해야한다는 것이다.

> A type that can be compared for value equality.         
값이 동일한 지 어떤지를 비교할 수 있는 타입

즉, Equatable 프로토콜을 준수하는 타입은 등호 연산자(==) 또는 같지 않음 연산자(!=)를 사용해 동등성을 비교할 수 있다. swift 표준 라이브러리의 대부분 기본 데이터타입은 Equatable을 따른다. 따라서 Equatable 프로토콜은 어떤 타입이 채택하는 것으로 이때 타입에는 클래스, 구조체, 열거형 등이 있을 것이다.

뿐만 아니라 Int, String, Double, Boole 등 기본적인 데이터타입도 Equatable을 따르고 있다.


```swift
var some = 1
var other = 2

if some == other {
  // code
} else {
  // code
}
```

위 코드는 Int 이지만 String, Double, Float에도 다 적용이 될 것이다.

이것이 가능한 이유는 결국 이 타입 모두 Equatable이라는 프로토콜을 따르고 있기 때문이다. 그렇기 때문에 이들이 똑같은지 아닌지를 확인할 수 있는 것이다. 그렇다면 만약 우리가 아주 복잡한 클래스나 구조체를 만들어 비교하고 싶다면 어떻게 해야할까?

즉, 클래스, 구조체 이들도 타입인데 이들이 가진 인스턴스를 비교하려면 어떻게 해야할까?


```swift
class A {
  var aNum : Int
  init(_ aNum: Int) {
    self.aNum = aNum
  }
}

if A(1) == A(2) // error
```

즉 xcode는 A(1)와 A(2)가 같은지 확인할 수 없다. <br>
A(1).aNum == A(2).aNum 이 같은지 아닌지는 확인할 수 있을지라도 말이다.

**A(1).aNum == A(2).aNum은 Int이고 Int는 Equatable을 따르고 있기 때문에 확인할 수 있다.**

근데 이때 나는 죽어도 A(1) == A(2) 이를 확인하고 싶다면 그떄 `Equatable`을 사용하는 것이다.

```swift
// 클래스A는 Equatable을 채택하고 준수함으로써 동일한지 안한지 판별이 가능해짐
class A: Equatable {
  var aNum : Int
  init(_ aNum: Int) {
    self.aNum = aNum
  }
}
```

<center>
<figure>
<img src="/assets/post-img/swift/49.png" alt="" width="50%">
</figure>
</center>

그러면 이때 위와 같은 에러가 발생할 것이다.

위 에러는 우리가 테이블뷰/컬렉션뷰 에서 자주 봤던 **클래스A가 Equatable 프로토콜을 준수하고있지 않다** 는것을 알려주는 에러다.

즉, Optional이 아닌 메소드가 있는 것으로 생각하면 쉽다.

```swift
public protovol Equatable {
  public static func ==(lhs: Self, rhs: Self) -> Bool {

  }
}
```

이제 해당 코드를 우리 클래스 코드에 복사 붙여넣기 해보자.

```swift
class A: Equatable {
  var aNum : Int
  init(_ aNum: Int) {
    self.aNum = aNum
  }
  public static func ==(lhs: Self, rhs: Self) -> Bool {

  }
}
```

그러면 또 아래와 같은 에러가 등장할 것이다.

<center>
<figure>
<img src="/assets/post-img/swift/50.png" alt="" width="50%">
</figure>
</center>

위 에러는 말 그대로 Self가 아니라 A인것같다고 말해주는 에러이다. 따라서 Self가 A이긴 하지만 이를 파라미터 안에서 사용하지 못하는 것으로 보이기에 A로 바꿔줘보자!

```swift
class A: Equatable {
  var aNum : Int
  init(_ aNum: Int) {
    self.aNum = aNum
  }
  public static func ==(lhs: A, rhs: A) -> Bool {

  }
}
```

<center>
<figure>
<img src="/assets/post-img/swift/51.png" alt="" width="50%">
</figure>
</center>

그러면 또 위와 같이 리턴값이 없다는 에러가 나오게 된다. 메소드 안에서 값을 비교해줘야 한다.

```swift
class A: Equatable {
  var aNum : Int
  init(_ aNum: Int) {
    self.aNum = aNum
  }
  public static func ==(lhs: A, rhs: A) -> Bool {
    return lhs.aNum == rhs.aNum
  }
}
```

즉, 클래스 A의 프로퍼티인 aNum이 값은지 같지 않은지를 판별해서 리턴해주도록 한다.

이때 aNum은 Int타입인데 Equatable을 준수하기 때문에 당연히 비교가 가능한 것이고 그 결과가 Bool로 나오게 되는 것이다.

이제 그럼 우리가 궁극적으로 원했던 비교를 해보자.

```swift
if A(1) == A(2) {
  print("same")
} else {
  print("different")
}

// different
```

매우 잘 되는 것을 볼 수 있을 것이다!

---
layout: post
title: swift 기본문법 - 제네릭(Generics)
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 제네릭(Generics)

제네릭은 자료형에 구애받지 않는 코드를 의미한다. 스위프트는 태생적으로 강한 타입의 언어이기 때문에 변수와 인자, 리턴값 등 모두 자료형의 구속을 받게 된다. 이것때문에 때로는 비생산적이고 반복적인 코딩을 해야할 때가 발생하는데 제네릭은 이 부분을 해결해주는 유용한 코드로서 스위프트의 가장 강력한 기능 중 하나이다.

사실 스위프트의 array이나 dictionary도 제네릭이다. 예로 array에는 Int들을 담을 수도 있고 String도 담을 수 있기 때문이다.

따라서 이 제네릭을 사용하게 되면 유연하고 재사용성 높은 코드를 작성할 수 있다는 장점이 있다.


자. 아래 두 정수의 값을 바꿔주는 함수가 있다고 하자.

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
  let temporaryA = a
  a = b
  b = temporaryA
}
```

그런데 아래 사진에서와 같이 파라미터에 Double형을 넣으면 에러가 뜨게 된다.<br>
swapTwoInts의 파라미터는 Int형이기 때문이다.


<center>
<figure>
<img src="/assets/post-img/swift/47.png" alt="" width="50%">
</figure>
</center>

그런데 우리가 만약에 같은 기능을 하는 함수임에도 불구하고 String, Double 등 각각 다양한 타입의 파라미터를 받아주는 함수를 매번 만들어야 한다고 생각해보자. 매우...매우!!!! 비효율적이라는 생각이 들것이다.

바로 이때 generic을 사용하면 된다.


```swift
func swapTwoInts<T>(_ a: inout: T, _ b: inout: T) {
  let temporaryA = a
  a = b
  b = temporaryA
}
```

이렇게만 해주면 이 함수 하나로 Int, String, Double 변수들의 값을 바꿔줄 수 있다. <br>
단 a, b가 모두 같은 타입일 때만 말이다!

그렇다면 이때 **T** 는 무엇을 의미할까?


### T 타입 파라미터

#### 1. 파라미터: 타입이 들어갈 부분에 T를 적었다.

이때 T는 Placeholder 타입 "이름"을 의미한다. String, Int타입도 이름으로 이 대신 T라는 **타입이름** 이 들어간것!<br>
이 T는 swapTwoInts라는 함수가 호출될 때마다 결정된다. 그리고 이때 a와 b는 이 T타입과 반드시 일치해야 한다.

#### 2. `<T>`

generic함수와 일반함수에서의 가장 큰 차이일 것이다.

제네릭함수는 함수 이름 옆에 위에서 말한 Placeholder 타입 이름이 온다. 대괄호(<>)로 묶어준 이유는 Swift에게 함수 정의 내 Placeholder 타입이름인 것을 알리기 위해서이다. 그냥 "T"라는 것은 Placeholder이므로, swiftsms "T"라는 실제 타입을 찾지 않는다.

따라서 아래와 같이 작성해도 문제 없다.

```swift
func swapTwoInts<zehye>(_ a: inout: zehye, _ b: inout: zehye) {
  let temporaryA = a
  a = b
  b = temporaryA
}
```

뿐만 아니라 `<T>`자리에는 여러개가 들어갈 수 있다(,로 구분)

그러나 swift는 매~우 안전한 언어이며 타입에 굉~장히 민감하기 때문에 a와 b의 타입은 반드시 같아야 한다. <br>
즉, Int와 String을 서로 바꿀 수 없게 하며, 만약 이렇게 하면 컴파일 에러가 뜰 것이다!


뿐만 아니라 swift에는 이미 **swap** 이라는 함수가 있는데 이또한 제네릭 함수이다.

```swift
public func swap<T>(_ a: inout T, _ b : inout T)
```


## 타입 제약

위에서 구현한 swapTwoInts 함수는 이제 제네릭으로 구현되어 있기 때무에 모든 타입에서 잘 동작할 것이다.

그러나 가끔 특정한 타입에서만 제네릭 함수를 사용하고 싶을때도 있을 것이다. 이를 위해 타입제약(Type Constraint)이 있다.<br>
타입 제약을 통해 타입 파라미터(Type Parameter, swapTwoInts에서는 T)가 특정 클래스로부터 상속되거나, 특정 프로토콜을 준수해야만 제네릭 함수를 쓸 수 있도록 제약을 걸 수 있다.

우리는 위에서 딕셔너리도 제네릭의 콜렉션이라는 것을 인지했다.

그런데 딕셔너리에서의 key에는 아무 타입이나 들어갈 것처럼 보이지만 사실 **Hashable 프로토콜** 을 준수해야만 Key로 들어올 수 있다.

```swift
public struct Dictionary<Key, Value> : Collection, ExpressibleByDictionaryLiteral where Key: Hashable {

}
```

즉, 우리가 이때까지 넣었던 Int, String, Double, Bool 등은 Hashable 프로토콜을 준수하고 있다는 것을 의미한다.

따라서 Dictionary는 타입 제약을 통해 특정한 클래스를 상속받거나, 특정한 프로토콜을 준수한 타입만이 들어올 수 있도록 구현되어 있다. 물론 제네릭을 통해서 말이다! 그럼 우리도 한번 만들어보도록 하자!


```swift
func somwFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
  .. function body goes here
}
```

위 코드는 타입 제약이 들어간 제네릭 함수의 syntax이다. 위 함수에서의 T,U모두 타입 파라미터이다. <br>
T 옆에는 특정 클래스, U 옆에는 특정 프로토콜이 존재하며 두개의 구분은 **,** 으로 하였다.

- T: SomeClass의 하위클래스여야 한다는 제약
- U: SomeProtocol을 준수해야한다는 제약


```swift
func findIndex(ofString valueToFind: String, in array: [String]) -> Int? {
  for (index, valiue) in array.enumerated() {
    if value == valueToFind {
      return index
    }
  }
  return nil
}
```

위 findIndex함수는 String 배열과 찾고싶은 String 하나를 파라미터로 주면 해당 String이 있으면 그 인덱스를 반화해주고 없으면 nil을 반환해주는 함수이다. 즉 [1,2,3,4,5,6]이라는 배열을 가지고 있는데 내가 찾고 싶은 Int가 몇번째 인덱스에 있는지 궁금할때 사용하는 함수를 의미한다.

위 함수를 이제 제네릭으로 바꿔보자!


```swift
func findIndex<T>(of valueToFind: T, in array: [T]) -> Int? {
  for (index, value) in array.enumerated() {
    if value == valueToFind {
      return index
    }
  }
  return nil
}
```

<center>
<figure>
<img src="/assets/post-img/swift/48.png" alt="" width="50%">
</figure>
</center>

그러면 위와 같은 오류가 발생할 것이다.

**value == valueToFind** 구문으로부터 발생하는 오류로 swift의 모든 타입이 저렇게 == 라는 여산자로 비교될 수 있는 것이 아니기 때문에 발생하는 에러이다. 즉, swift가 가능한 모든 타입 T에 대해 이 코드가 작동한다는 것을 보장할수 없기 때문에 컴파일 에러가 발생하는 것이다!

이때 사용할 개념이 **Equatable** 이다.


```swift
func findIndex<T: Equatable>(of valueToFind: T, in array: [T]) -> Int? {
  for (index, value) in array.enumerated() {
    if value == valueToFind {
      return index
    }
  }
  return nil
}
```

즉, Equatable을 준수하는 모든 타입은 위 findIndex 함수를 안전하게 사용할 수 있게 된다.

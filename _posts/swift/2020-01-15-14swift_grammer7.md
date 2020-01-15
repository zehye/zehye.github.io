---
layout: post
title: swift 기본문법 - 함수 고급
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
인프런, 야곰의 스위프트 기본문법 강좌를 듣고 정리하였습니다.

<hr>

## 매개변수 기본값

기본값을 갖는 매개변수는 매개변수 목록 중에 뒤쪽에 위치하는 것이 좋다.

```
func 함수이름(매개변수1 이름: 매개변수1 타입, 매개변수2 이름: 매개변수2 타입 = 매개변수 기본값 ...) -> 반환타입 {
  함수 구현부
  return 반환값
}
```

예시는 아래와 같다.

```swift
func greeting(friend: String, me: String = "zehye") {
  print("hello \(friend)! I'm \(me)")
}

// 뿐만 아니라 매개변수 기본값을 가지는 매개변수는 생략이 가능하고 수정도 가능하다.
greeting(friend: "hana")
greeting(friend: "John", me: "eric")
```


### 전달인자 레이블

전달인자 레이블은 함수를 호출할 때 매개변수의 역할을 좀 더 명확하게 하거나 함수 사용자의 입장에서 표현하고자 할 때 사용한다.

```
func 함수이름(전달인자 레이블 매개변수1 이름: 매개변수1 타입, 전달인자 레이블 매개변수2 이름: 매개변수2 타입 ...) -> 반환타입 {
  함수 구현부
  return 반환값
}
```

예시는 아래와 같다.

```swift
func greeting(to friend: String, from me: String) {
  print("Hello \(friend)! I'm \(me)")
}
```

위와 같이 함수의 중복 정의 또한 손쉽게 할 수 있다.<br>
이미 greeting이라는 함수가 있었음에도 불구하고 `to`, `from`이라는 레이블을 이용해 함수 이름이 바뀐 효과를 갖는다.

함수의 외부에서는 **to** 라는 전달인자 레이블을 통해 함수를 호출하게 되고, 함수 내부에서는 **friend** 라는 매개변수 이름을 통해 매개변수를 사용하게 된다.<br>
이렇듯 함수외부에서는 꼭 전달인자 레이블을 사용해야하며, 함수 내부에서는 매개변수 이름을 사용해주어야 한다.
```swift
greeting(to: "hana", from: "zehye")
```


### 가변 매개변수

매개변수를 통해 전달되어올 값들의 개수를 알기 어려울때 사용한다. 가변 매개변수는 함수당 하나만 가질 수 있다.

```
func 함수이름(매개변수1 이름: 매개변수1 타입, 전달인자 레이블 매개변수2 이름: 매개변수2 타입...) -> 반환타입 {
  함수 구현부
  return
}
```

예시는 아래와 같다.

```swift
func sayHelloToFriends(me: String, friends: String...) -> String {
  return ("Hello \(friends)! I'm \(me)")
}
```

이때 전달인자가 없거나 nil을 넣어주게 되면 오류가 발생한다.

```swift
print(sayHelloToFriends(me: "zehye", friend: ))
print(sayHelloToFriends(me: "zehye", friend: nil))
```

만약 가변 인자에 아무값도 넘겨주고 싶지 않다면 전달인자 레이블을 생략하면 된다.

```swift
print(sayHelloToFriends(me: "zehye"))
```


### 데이터 타입으로서의 함수

- 스위프트는 함수형 프로그래밍 패러다임을 포함하는 다중 패러다임언어이다.
- 스위프트의 함수는 일급객체이므로 변수, 상수 등에 저장이 가능하고 매개변수를 통해 전달 또한 가능하다.

그래서 스위프트의 함수는 하나의 데이터 타입으로서 표현이 될 수 있다.

```
(매개변수1 타입, 매개변수2 타입) -> 반환타입  // 반환타입은 생략 불가능
```

```swift
// 해당 변수에 함수가 들어올 것이라고 선언
var someFunction: (String, String) -> Void = greeting(to:from:)
someFunction("eric", "hana")

someFunction = greeting(friend:me:)
someFunction("eric", "hana")

// 타입이 다른 함수는 할당 불가능, 가변매개변수로 오는 함수
someFunction = sayHelloToFriends(me: friends:)

func runAnother(function: (String, String) -> Void) {
  function("jenny", "mike")
}

runAnother(function:greeting(friend:me:))
// 변수에 담겨있는 함수를 넘겨줄 수 있다
runAnother(function: someFunction)
```

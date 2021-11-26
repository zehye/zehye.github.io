---
layout: post
title: swift - ==와 ===
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## ==

`a == b`: a값과 b값이 같은 지 비교

```swift
let v1 = 1
let v2 = 2

print(v1 == v2) // false
```

## ===

`a === b`: a가 참조하고 있는 인스턴스와 b가 참조하고 있는 인스턴스가 같은 지 비교

swift에서는 크게 값, 참조타입이 존재하는데 참조타입을 가질 수 있는 클래스 객체를 참조할 수 있는 변수는 다수가 될 수 있기 때문에 === 연산자가 필요하다.

```swift
let c1 = Person(id: 1, name: "zehye")
let c2 = Person(id: 1, name: "jihye")
let c3 = c1

print(p1 === p2)  // false
print(p1 === p3)  // true
```

<hr>

클래스의 인스턴스는 heap 영역에 할당되며 인스턴스를 참조하는 변수는 stack영역에 할당된다.<br>
그래서 `===`는 heap 영역의 값을 비교하고 `==`는 stack영역의 값을 비교한다.

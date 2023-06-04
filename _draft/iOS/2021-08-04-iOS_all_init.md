---
layout: post
title: required init과 failable init? 
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## required initializer

required를 사용하면 서브클래스에서 반드시 동일한 생성자를 직접 구현해야 한다.

```swift
class Person {
  var name: String

  required init(name: String) {
    self.name = name
  }

  func getName() {
    print("\name")
  }
}

class Animal: Person {
  var age: Int

  init() {
    age = 1
    super.init()
  }

  required init(name: String) {
    age = 1
    super.init(name: name)
  }
}
```



## failable initializer

실패를 허용하는 이니셜라이저<br>
초기화에 실패하더라도 에러가 발생하지 않고 대신 nil을 반환! > 생성자의 옵셔널 버전이라고 생각하자.

```swift
struct Position {
  let x: Double
  let y: Double

  init?(x: Double, y: Double) {
    guard x >= 0.0, y >= 0.0 else { return nil }
    self.x = x
    self.y = y
  }

  init!(value: Double) {
    guard value >= 0.0 else { return nil }
    self.x = value
    self.y = value
  }
}
```

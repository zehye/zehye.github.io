---
layout: post
title: Swift - Mutating이란?
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Mutating

구조체의 메서드가 구조체 내부에서 데이터 수정될때는 `Mutating` 키워드를 선언해야함

```swift
struct Point {
  var x = 0
  var y = 0

  mutating func moveTo(x: Int, y: Int) {
    self.x = x
    self.y = y
  }
}
```

즉, 다른 구조체안에서 Mutating이 있느냐 없으냐에 따라 원래 구조체 내부의 값을 변경하는 api인지 아닌지 유추가 가능하다. 

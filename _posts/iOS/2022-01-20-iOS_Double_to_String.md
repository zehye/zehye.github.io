---
layout: post
title: iOS Double형 데이터를 String으로 나타내는 법(+ 소수점 표현하는 방법)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 문제상황

현재 서버에서는 Double형 데이터가 넘어오고 있다. > `let double = 12.012312` 이런식으로<br>
근데 나는 이를 String으로 변환하면서 소수점 두 번째 자리까지만 보여주고 싶다.

추가로 뒤에 %까지 추가하고 싶다면?

```swift
let double: Double = 12.012312
var string: String

string = String(format: "%.2f", double) + "%"
print(string)
// 12.01% 출력됨
```

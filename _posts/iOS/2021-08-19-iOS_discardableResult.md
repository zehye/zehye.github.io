---
layout: post
title: iOS discardableResult이란?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## @discardableResult

버릴수 있는 결과 라는 뜻으로 말 그대로 스위프트에서는 개발자를 위해 여러가지 warning들을 띄워준다.

- 값이 변하지 않으면 var가 아닌 let을 사용하세요
- 변수가 사용되지 않았으니 _ 를 사용해세요

등등...

이와 마찬가지로 **Result of vall to ~~ is unused** 와 같이 결과 값이 사용되지 않았습니다. 라는 워닝도 있다.<br>
그러나 결과를 return 하는데 이 결과가 필요 없는 경우도 있을것! 그럴때 warning을 보고싶지않다? 이럴때 사용한다.


```swift
@discardableResult
func log (_ msg: String) -> String {
  return "\(msg) is enetered"
}
```

즉, **@discardableResult** 는 결과와 관계없이 warning을 띄워주지마! 라는 의미를 가진다.

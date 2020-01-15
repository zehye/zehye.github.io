---
layout: post
title: swift 기본문법 - Any, AnyObject, nil
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
인프런, 야곰의 스위프트 기본문법 강좌를 듣고 정리하였습니다.

<hr>

기본 데이터타입은 아니지만, 데이터 타입의 위치에서 편한 역할을 수행하는 Any, AnyObject, nil

### Any

swift에서 모든 타입을 지칭하는 키워드<br>
데이터 타입 위치에 들어올 수 있고, 모든 타입을 수용할 수 있다. 어떤 타입의 데이터도 다 들어오는 것이 가능하다.

```swift
var someAny: Any = 100
someAny = "어떤 타입도 수용 가능하다"
someAny = 123.12

let someDouble: Double = someAny
```

그러나 Any타입은 Double에 할당할 수 없다.

<center>
<figure>
<img src="/assets/post-img/swift/17.png" alt="" width="80%">
</figure>
</center>


### AnyObject

모든 클래스 타입을 지칭하는 프로토콜 > 클래스 인스턴스만을 취급

```swift
class SomeClass = {}
var someAnyObject: AnyObject = SomeClass()
```


### nil

없음을 의미하는 키워드

어떤 데이터타입이든 들어올 수 있지만 nil은 들어갈 수 없다.<br>
nil은 NULL과 유사한 표현으로 보면 된다.

<center>
<figure>
<img src="/assets/post-img/swift/18.png" alt="" width="80%">
</figure>
</center>

nil이 필요한 순간은 옵셔널에서 볼 수 있다.

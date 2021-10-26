---
layout: post
title: get, set, willset, didset 이란?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## get, set

swift내에서 get, set은 우리가 일반적으로 알고있는 getter, setter와 매우 유사하다.

```swift
var test: Int {
  get {
    return test
  }
  set (value) {
    test = value
  }
}

print(test)  // test의 value 출력
test = 123   // value = 123
```

이렇게 코드를 작성하면 아마 경고? 혹은 에러가 발생할 것이다.<br>
get, set이 해당 프로퍼티에 직접적으로 연결되어 있기 때문에 get {}, set {} 에서 test에 접근하면 recursive하게 자기 자신의 get, set이 호출되기 때문에 위와 같은 방법은 사용하지 않는다.
따라서 이러한 점을 보완하기 위해 `저장소`라는 개념을 이용한다. 여기서 저장소는 get, set을 사용할 때 ㅇ녀산된 값을 저장할 변수를 의미한다.


```swift
var _text: Int  // 실제 값이 저장될 변수

var test: Int {
  get {
    return _test
  }
  set (value) {
    _test = value
  }
}
```

외부에서 test의 값에 접근하거나 새로운 값이 할당될 경우 실제 값이 저장되는곳은 _test이다.<br>
즉, test는 _test에 값을 저장하기 위해 일차적으로 연산의 결과는 저장하는 저장소가 된다.

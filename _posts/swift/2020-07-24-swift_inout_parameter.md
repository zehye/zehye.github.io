---
layout: post
title: swift - inout parameter
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Inout Parameter

Inout은 함수에서 직접 파라미터 값에 접근할 수 있도록 해주는 기능이다.

즉, 파라미터로 변수의 주소값을 넘겨 직접 접근할 수 있도록 해주는 기능으로 사용법은 아래와 같다.

```swift
func swap(_ a: inout Int, _ b: inout Int) {
  let temp = a
  a = b
  b = temp
}

var x = 15
var y = 47

swap(&x, &y)
print(x) // 47
print(y) // 15
```

함수에 주소값을 넘기기 위해서는 위처럼 `inout` 키워드를 데이터 타입 앞에 써주어야 한다. 그리고 함수를 호출할 때 변수의 값을 넣는 것이 아니라 `&변수명`을 넣어주어야 한다. 이때 inout을 사용한 함수는 일반 swift 함수와 다른점이 있다.

swift 함수는 기본적으로 함수의 모든 인자가 함수를 호출할 때 상수로 호출되기 때문에 넘어온 값을 바꿀 수 없다는 것이다.

```swift
func swap(_ a: Int, _ b: Int) {
  let temp = a
  a = b // error
  b = temp
}

func swap2(_ a: Int, _ b: Int) {
  var a = a //mutable하게 변경  
  a = b
  b = temp
}
```

즉, 위와 같이 a의 값이 `immutable`하기 때문에 컴파일에러가 발생하게 되고 그래서 a의 값이 바뀔수 있도록 허용하기 위해 swap2 함수와 같이 `var a = a` 를 써주어야 한다. 하지만 inout을 사용한다는 것은 기본적으로 값을 변경하겠다는 의미를 내포하고 있기 때문에 `inout의 파라미터는 기본적으로 mutable 하다.` 

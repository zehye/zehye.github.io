---
layout: post
title: swift 기본문법 - 조건문과 반복문
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
인프런, 야곰의 스위프트 기본문법 강좌를 듣고 정리하였습니다.

<hr>

## 조건문

### if-else

```
if condition {
  statements
} else if condition {
  statements
} else {
  statements
}
```
예시는 아래와 같다.

```swift
if someInteger < 100 {
  print("100 미만")
} else if someInteger > 100 {
  print("100 이상")
} else {
  print("100이다!")
}
```

스위프트의 조건에는 항상 bool타입이 들어와야 한다.
someInteger는 bool타입이 아닌 Int 타입이기 때문에 컴파일 오류가 발생한다.


### Switch

```
switch value {
case pattern:
  code
default:
  code
}
```

스위프트에서는 **범위연산자** 라는게 등장하는데 이를 활용하면 더욱 쉽고 유용하게 사용가능하다.

```swift
switch someInteger {
case 0:
  print("zero")
case 1..<100:
  print("1~99")
case 100:
  print("100")
case 101...Int.max:
  print("over 100")
default:
  print("unknown")
}
```

그리고 정수 외의 대부분의 기본 타입을 사용할 수 있다.<br>
문자열을 가지고 비교값에 넣어주고 case구문을 작성할 수 있다.

```swfit
switch "zehye" {
case "jake", "mina":
  print("jake")
case "zehye":
  print("zehye")
default:
  print("unknown")
}
```

그런데 스위프트의 switch 구문에서는 명확히 case가 다 명시되지 않는 한 default 구문을 반드시 작성해주어야 한다.
그리고 case구문 다음에 break이 없는데, 명시적으로 break를 하지 않아도 break가 걸린다.

혹은 break가 걸리지 않게 하고 싶다면 `fallthrough`를 사용해주면 된다.

```swfit
switch "zehye" {
case "jake":
  print("jake")
  fallthrough
case "mina":
  print("mina")
case "zehye":
  print("zehye")
default:
  print("unknown")
}
```



## 반복문

반복문은 컬렉션 타입과 많이 사용되므로 미리 변수와 상수를 만들어놓도록 한다.

```swift
var integers = [1,2,3]
let people = ["zehye": 10, "eric": 15, "mike": 12]
```

### for-in

```
for item in items {
  code
}
```

예시는 아래와 같다.

```swift
for integer in integers {
  print(integer)  // [1,2,3]
}

// Dictionary의 item은 key와 value로 구성된 튜플타입이다.
for (name, age) in people {
  print("\(name): \(age)")
}
```


### while

```
while condition {
  code
}
```

예시는 아래와 같다.<br>
조건문과 같이 bool값이 들어와야 한다.

```swift
while integers.count > 1 {
  integers.removeLast()
}
```


### repeat-while

다른 언어의 do~while과 같다.<br>
while문과 비슷하지만 반복문 내의 코드를 우선 한번은 실행 한 뒤 조건문을 테스트하는 점이 차이점이다.


```
repeat {
  code
} while condition
```

예시는 아래와 같다.

```swift
repeat {
  integers.removeLast()
} while integers.count > 0
```

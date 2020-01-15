---
layout: post
title: swift 기본문법 - 옵셔널(Optional)
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
인프런, 야곰의 스위프트 기본문법 강좌를 듣고 정리하였습니다.

<hr>


## Optional

값이 '있을 수도, 없을 수도 있음'

```swift 
let optionalConstant: Int? = nil  //혹은
let optionalConstant: Optional<Int> = nil

let someConstant: Int = nil  // 컴파일 에러 발생
```

<center>
<figure>
<img src="/assets/post-img/swift/19.png" alt="" width="80%">
</figure>
</center>

옵셔널이 아닌 상수에 nil값을 할당하려고 하면 컴파일 오류가 발생한다.


### Optional은 왜 필요한가?

nil의 가능성을 명시적으로 표현해주는 것

- nil의 가능성을 문서화하지 안아도 코드만으로 충분히 표현 가능
  - 문서/주석 작성 시간을 절약
- 전달받은 값이 옵셔널이 아니라면 nil체크를 하지 않더라도 안심하고 사용 가능
  - 효율적인 코딩이 가능해짐
  - 예외 상황을 최소화하는 안정된 코딩 가능


```swift
func someFunction(someOptionalParam: Int?) {
  // ...
}

func someFunction(someOptionalParam: Int) {
  // ...
}

someFunction(someOptionalParam: nil)
someFunction(someOptionalParam: nil)  // 컴파일 에러 발생
```

<center>
<figure>
<img src="/assets/post-img/swift/19.png" alt="" width="80%">
</figure>
</center>

컴파일 단계에서도 안전하게 Optional값을 처리 가능하다.


### Optional의 표현방식 > ?, !

#### Implicitly Unwrapped Optional, 암시적 추출 옵셔널 > !

타입 뒤에 !를 표현하는 것

옵셔널 타입 자체는 열거형이기 때문에 switch구문으로 표현이 가능하다.


```swift
var optionalValue: Int! = 100

switch optionalValue {
case .none:  // 값이 없다
  print("This Optional variable is nil!!")

case .some(let value):  // 값이 들어왔다
  print("Value is \(value)")
}

// 기존 변수처럼 사용 가능
optionalValue = optionalValue + 1

// nil 할당 가능
optionalValue = nil

// 잘못된 접근으로 인한 런타임 오류 발생, 이미 nil값을 할당했기 때문
optionalValue = optionalValue + 1
```


#### 일반적인 옵셔널 > ?

```swift
var optionalValue: Int? = 100

switch optionalValue {
case .none:  // 값이 없다
  print("This Optional variable is nil!!")

case .some(let value):  // 값이 들어왔다
  print("Value is \(value)")
}

// nil 할당 가능
optionalValue = nil

// 기존 변수처럼 사용불가 - 옵셔널과 일반 값은 다른 타입이기 때문에 연산불가
optionalValue = optionalValue + 1
```

<center>
<figure>
<img src="/assets/post-img/swift/20.png" alt="" width="80%">
</figure>
</center>


### Optional Unwrapping, 옵셔널 값 추출

옵셔널 값을 어떻게 꺼내서 쓰고 활용하는가?

- 옵셔널 바인딩(Optional Binding)
- 강제 추출(Force Unwrapping)


#### 옵셔널 바인딩(Optional Binding)

옵셔널 값을 꺼내오는 방법 중 하나로 `nil체크 + 안전한 값 추출`

<center>
<figure>
<img src="/assets/post-img/swift/21.jpeg" alt="" width="80%">
</figure>
</center>

값이 있는지 확인해보고 값이 있다면 꺼내오고 없다면 지나간다.

```swift
func printName(_ name: String) {
  print(name)
}

var myName: String? = nil

// 전달되는 값의 타입이 다르기 때문에 컴파일 오류 발생
printName(myName)
```

전달되는 값의 타입이 다를때마다 컴파일 오류가 발생하기 때문에 `if-let`을 통해 옵셔널 바인딩을 하도록 한다.

```swift
func printName(_ name: String) {
  print(name)
}

var myName: String! = nil

if let name: String = myName {
  printName(name)
} else {
  print("myname == nil")
}

// name이라는 상수는 if-let 구문안에서만 사용이 가능하다.
// 구문 바깥에서 사용했기 때문에 컴파일 오류가 발생한다.
printName(myName)
```

그리고 여러 옵셔널 변수들을 바인딩할 수 있다. 그러나 해당 변수들에 모두 값이 들어가있어야 한다.

```swift
var myName: String? = "zehye"
var youtName: String? = nil

if let name = myName, let friend = youtName {
  print("\(name) and \(friend)")  // youtName이 nil이기 때문에 실행이 안됨
}

yourName = "hana"
if let name = myName, let friend = youtName {
  print("\(name) and \(friend)")  // 실행됨
}
```


#### 강제 추출(Force Unwrapping)

옵셔널의 값을 강제로 추출

```swift
func printName(_ name: String) {
  print(name)
}

var myName: String? = "zehye"
printName(myName!)  // zehye !를 사용함으로써 값을 강제로 추출

myName = nil
print(myName!)  // 강제추출 시 값이 없으므로 런타임 오류 발생

var yourName: String! = nil  // 처음 선언시 사용 가능
printName(yourName)  // 여기서 ! 사용하지 않았어도 강제추출 가능
```

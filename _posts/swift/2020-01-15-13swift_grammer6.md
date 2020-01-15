---
layout: post
title: swift 기본문법 - 함수 기본
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
인프런, 야곰의 스위프트 기본문법 강좌를 듣고 정리하였습니다.

<hr>

## 함수의 선언

### 함수의 기본 형태

```
func 함수이름(매개변수1 이름: 매개변수1 타입, 매개변수2 이름: 매개변수2 타입 ...) -> 반환타입 {
  함수 구현
  return 반환값
}
```

간단하게 구현해보면 아래와 같다.

```swift
func sum(a: Int, b: Int) -> Int {
  return a + b
}
```


### 함수의 반환값이 없는 경우

함수의 반환값이 없는 경우 `void`를 사용한다 <br>
void는 **없다** 라는 표현의 타입 별칭이다.

```
func 함수이름(매개변수1 이름: 매개변수1 타입, 매개변수2 이름: 매개변수2 타입 ...) -> Void {
  함수 구현
  return
}
```

예시는 아래와 같다.

```swift
func printMyName(name: String) -> Void {
  print(name)
}
```

혹은 void는 생략도 가능하다.

```
func 함수이름(매개변수1 이름: 매개변수1 타입, 매개변수2 이름: 매개변수2 타입 ...) {
  함수 구현
  return
}
```

예시는 아래와 같다.

```swift
func printMyName(name: String)  {
  print(name)
}
```

### 함수의 매개변수가 없는 경우

```
func 함수이름() -> 반환타입 {
  함수 구현부
  return 반환값
}
```

```swift
func maximumIntegerValue() -> Int {
  return Int.max
}
```


### 함수의 매개변수와 반환값이 없는 경우

```
func 함수이름() -> {
  함수 구현부
}

혹은

func 함수이름() {
  함수 구현부
}
```

```swift
funcA() { print("bye")}
```


### 함수의 호출

```swift
sum(a:3, b:5)
printMyName(name: "zehye")
maximumIntegerValue()
```

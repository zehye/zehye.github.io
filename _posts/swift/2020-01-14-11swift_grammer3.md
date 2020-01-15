---
layout: post
title: swift 기본문법 - 기본 데이터타입
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
인프런, 야곰의 스위프트 기본문법 강좌를 듣고 정리하였습니다.

<hr>

## 기본 데이터타입

Bool, Int, UInt, Float, Double, Character, String


### Bool

```swift
var someBool: Bool = true
```

### Int

64비트 정수형 타입 > 양수, 음수, 0 포함

```swift
var someInt: Int = 100
```


### UInt

부호가 없는 정수 > 양의 정수

```swift
var someUInt: UInt = 100
```


### Float

32비트 부동소수형 타입

```swift
var someFloat: Float = 3.14
```


### Double

64비트 부동소수형 타입

```swift
var someDouble: Double = 3.14
```


### Character

한글자 문자를 표현하기 위한 타입 > 유니코드를 사용

```swift
var someCharacter: Character = "👻"
```


### String

```swift
var someString: String = "하하하 👻"
someString = someString + "웃으면 복이와요!!!"
```

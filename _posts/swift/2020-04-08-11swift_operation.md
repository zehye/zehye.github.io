---
layout: post
title: 기본연산자 정의(Basic Operators)
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

파이썬으로 처음 개발 공부를 시작했던 나에게 초반 머리에 잘 들어오지 않던 연산자 개념이 있었다. 그것은 바로 `범위연산자`<br>
어떤때에는 .. 어떤때에는 ... 이게 무슨의미지 헀는데, 막상 찾아보니 별거아닌(?) 개념이어서 그냥 간단하게 정리해보겠습니다 :)


## 기본연산자

swift에서는 통상적으로 이용하는 +, -, /, % 등과 같은 산술연산자와 $$과 같은 논리연산자 뿐만 아니라 a..<b, a...b와 같이 값의 범위를 지정할 수 있는 범위연산자를 지원한다.

- 단항 연산자: -a, !b, c!와 같이 하나의 대상 앞뒤에 바로 붙여 사용하는 연산자
- 이항 연산자: 2 + 3과 같이 두 대상 사이에 위치하는 연산자
- 삼항 연산자: a? b : c 형태로 swift에 삼항연산자는 이 연산자 단 하나만 존재


### 할당연산자(Assignment Operators)

할당 연산자는 값을 초기화시키거나 변경 > 상수, 변수 모두에 사용 가능하다.

```swift
let a = 10
var b = 5

b = a  // b의 값은 10

let (x, y) = (1, 2)  // 튜플을 이용해 한번에 할당 가능
```


### 사칙 연산자(Arithmetic Operators)

- 더하기, 뺴기, 곱하기, 나누기  + 나머지

```swift
1 + 2  // 3
5 - 3  // 2
2 * 3  // 6
10.0 / 2.5  //4.0
9 * 4  // 1
```


### 비교 연산자(Comparison Operators)

- 같다 (a == b)
- 같지 않다 (a != b)
- 크다 (a > b)
- 작다 (a < b)
- 크거나 같다 (a >= b)
- 작거나 같다 (a <= b)


#### 삼항 조건 연산자(Ternary Conditional Operator)

삼항 조건 연산자는 `question ? answer1 : answer2`의 구조를 갖는다. 그래서 question 조건이 참이면 answer1, 거짓인 경우 answer2가 실행된다.

```swift
if question {
  answer1
} else {
  answer2
}
```


#### Nil 병합 연산자(Nil-Coalescing Operator)

nil 병합 연산자는 `a ?? b`형태를 갖는 연산자다. 옵셔널 a를 벗겨서(unwrap) 만약 a가 nildlaus b를 반환한다.

```swift 
a != nil ? a! : b
```

즉, 옵셔널 a가 nil이 아니면 a를 unwrap하고 nil이면 b를 반환한다는 의미!


### 범위 연산자(Range Operators)

- 닫힌 범위 연산자: `a..b` 의 형태로 범위의 시작과 끝이 있는 연산자
- 반 닫힌 범위 연산자: `a..<b`의 형태로 a부터 b보다 작을때까지의 범위를 갖는 연산자 >> a부터 b-1까지의 값
- 단방향 범위: `[a..] 혹은 [..a]`의 형태로 범위의 시작 혹은 끝만 지정해 사용하는 연산자


### 논리 연산자(Logical Operators)

- 논리부정: `NOT(!a)`
- 논리 곱: `AND(a&&b)`
- 논리 합: `OR(a||b)`

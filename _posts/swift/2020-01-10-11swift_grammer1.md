---
layout: post
title: swift 기본문법 - 이름짓기, 콘솔로그, 문자열 보간법
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
인프런, 야곰의 스위프트 기본문법 강좌를 듣고 정리하였습니다.

<hr>

## 이름짓기 규칙

- Lower Camel Case: function, method, variable, constant > someVariableName..
- Upper Camel Case: type(class, struct, enum, extension) > Person, Point, Week..


## 콘솔로그

- print: 단순 문자열 출력
- dump: 인스턴스의 자세한 설명(description 프로퍼티)까지 설명


## 문자열 보간법(String Interpolation)

프로그램 실행 중 문자열 내에 변수 또는 상수의 실질적인 값을 표현하기 위해 사용 > `\()`

```swift
import Swift
let age: Int = 10

"안녕하세요! 저는 \(age)살 입니다."  // "안녕하세요! 저는 10살 입니다."
"안녕하세요! 저는 \(age+5)살 입니다."  // "안녕하세요! 저는 15살 입니다."

print("안녕하세요! 저는 \(age)살 입니다.")
```

```consol
>>> 안녕하세요! 저는 10살입니다
```

```swift
class Person {
  var name: String = "zehye"
  var age: Int = 26
}

let zehye: Person = Person()
print(zehye)
dump(zehye)
```

```consol
>>> Person 클래스 주소  // print의 결과값
>>> Person 클래스 주소  // dump의 결과값
>>> zehye
>>> 26
```

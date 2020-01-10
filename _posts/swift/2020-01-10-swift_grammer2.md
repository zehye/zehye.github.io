---
layout: post
title: swift 기본문법 - 상수와 변수
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
인프런, 야곰의 스위프트 기본문법 강좌를 듣고 정리하였습니다.

<hr>


## 상수와 변수

swift는 함수형 프로그래밍 패러다임을 지향하기에 불변 객체를 굉장히 중요시함으로 상수 표현이 많이 등장하게 된다.


- 상수 선언 키워드: **let**
  - 상수의 선언/ let 이름: 타입 = 값
- 변수 선언 키워드: **var**
  - 변수의 선언/ var 이름: 타입 = 값

swift는 띄어쓰기에 굉장히 민감한 언어로 이를 잘못 쓰게되면 오류/컴파일에 문제가 생길 수 있다.<br>
뿐만 아니라 값의 타입이 명확하다면 타입은 생략이 가능하다.

- let 이름 = 값
- var 이름 = 값

상수(let)으로 값을 지정해주고 나면 차후에 다른 값으로 변경이 불가능하다. 반면 변수(var)는 차후에 다른 값을 할당해줄 수 있다.

```swift
var variable : String = "차후에 변경이 가능한 var"
let constant : String = "차후에 변경이 불가능한 let"

variable = "변수는 이렇게 차후에 다른값을 할당할 수 있습니다."
constant = "상수는 차후에 값을 변경할 수 없습니다."
```

<center>
<figure>
<img src="/assets/post-img/swift/15.png" alt="" width="80%">
<figcaption>constant에는 값을 새로 할당할 수 없으니 let을 var로 변경하라.</figcaption>
</figure>
</center>




### 상수와 변수 선언 후 나중에 값 할당하기

나중에 할당하려고 하는 상수나 변수는 우선적으로 타입은 미리(반드시) 명시해주어야 한다.

```swift
let sum: Int  // 타입만 먼저 선언해놓은 상수
let inputA = 10
let inputB = 20

sum = inputA + inputB  // 상수 선언 후 첫 할당
```

상수는 선언 후 처음으로 한번만 할당해줄 수 있으며 할당 이후에는 그 값을 절대 바꿀 수 없다. <br>
변수또한 선언을 미리 할 수 있으며 상수와 다르게 할당 이후에도 그 값을 바꾸는 것이 가능하다.

```swift
var nickname : String

nickname = "zehye"
nickname = "jihye"
```

그리고 이러한 상수와 변수 모두 할당되기 전에 사용하고자 한다면 컴파일러가 오류를 띄워준다.


<center>
<figure>
<img src="/assets/post-img/swift/16.png" alt="" width="80%">
<figcaption>초기화 되기 이전에 사용이 되었다.</figcaption>
</figure>
</center>

---
layout: post
title: swift - init과 convenience init
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Designated init과 convenience init

우선 designated init을 swift의 초기화 이니셜 라이저를 의미한다. 이 init은 클래스의 모든 프로퍼티가 초기화될 수 있도록 해줘야 한다. 이름은 **designated init(지정 이니셜라이저)** 이지만 우리는 흔히 init이라고 쓴다.


```swift
class Person {
  var name: String
  var age: Int

  init(name: String, age:Int) {
    self.name = name
    self.age = age
  }
}
```

만약 init 파라미터에 클래스 프로퍼티가 하나라도 빠지게 되면 에러가 발생하게 된다.

<center>
<figure>
<img src="/assets/post-img/swift/53.png" alt="" width="80%">
</figure>
</center>


그렇다면 convenience init은 무엇인가?


### Convenience init

엄밀히 말하면 convenience init은 **보조 이니셜라이저** 이다. <br>
클래스의 원래 이니셜라이저인 init을 도와주는 역할을 하는 것이다.

designated init의 파라미터 중 일부를 기본값으로 설정해 이 convenience init안에 designated init을 호출하여 초기화를 진행할 수 있다. 그렇기 때문에 **convenience init을 사용하기 위해서는 designated init이 반드시 먼저 선언되어야 한다.**

swift의 이니셜라이저 규칙에는 몇가지가 있는데 그중에는

> convenience init은 같은 클래스에서 다른 이니셜라이저를 호출해야한다.

라는 규칙이 있다. convenience init의 예시를 살펴보자.


```swift
class Person {
  var name: String
  var age: Int
  var gender: String

  init(name: String, age: Int, gender: String) {
    self.name = name
    self.age = age
    self.gender = gender
  }

  convenience init(age: Int, gender: String) {
    self.init(name: "zehye", age: age, gender: gender)
  }
}
```

이렇게 적어주어도 전혀 오류가 나질 않는다.<br>
즉, 파라미터로 넘겨주지 않은 값은 그냥 임의로 지정해주고 파라미터로 넘어간 것들만 넣어주면 되는 것이다.

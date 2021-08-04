---
layout: post
title: static과 class 제대로 알아보자
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


- 함수: func이라는 키워드로 생성하는 것 모두 함수
- 메서드: 클래스, 구조체, 열거형 속에 포함되는 함수


## instance method

인스턴스와 관련된 메서드로 `인스턴스를 생성해야만` 호출이 가능하다.

```swift
class Person {
  func getName() {
    print("\(name)")
  }
}
```

이렇게 아무런 수식어 없이 func 으로 시작하는 메서드 모두 인스턴스 메서드라고 한다.<br>
이러한 인스턴스 메서드를 호출하기 위해서는 아래와 같은 방법을 사용한다.

1. Person이라는 인스턴스를 먼저 생성하고
2. 생성된 인스턴스에 .을 사용해 메서드에 접근한다.

즉, 인스턴스 메서드는 인스턴스와 연관된 메서드이기 때문에, 인스턴스를 생성해야만 인스턴스를 통해 메서드에 접근할 수 있으며 아무런 수식어없이 func으로 선언된 메서드는 모두 인스턴스 메서드를 의미한다.



## type method

형식(type)과 관련된 메서드로, `인스턴스 생성없이 형식(Type)의 이름만 알면` 호출이 가능하다.<br>
메서드 선언시 func 이란 키워드 앞에 `static`혹은 `class`가 붙으면 그 메서드는 타입 메서드이다.

```swift
class Person {
  static func getName() {
    print("\(name)")
  }

  class func getAge() {
    print("\(age)")
  }
}
```

위와 같은 메서드는 모두 타입 메서드이다.<br>
이러한 타입메서드를 호출하기 위한 방법은 아래와 같다.

1. 타입메서드는 인스턴스와는 전혀 상관없기때문에 Person이라는 Type에만 연관되어있다.
2. 따라서 Person이라는 type 이름을 호출한다.

```swift
Person.getName()
Person.getAge()
```

타입메서드는 인스턴스 메서드가 아니기 때문에, 인스턴스를 생성할 필요도 없으며<br>
오로지 자신이 속해있는 type의 이름인 Person만 알면 그 Person을 통해 메서드를 호출할 수 있게 된다.



### static? class?

타입메서드를 작성하기 위해서는 메서드 앞에 static 혹은 class를 적어주면 되는데,<br>
이둘을 적어주는 궁극적인 차이는 `메서드 오버라이딩(override)`의 여부이다.

**1. static** : 오버라이딩을 금지함

static은 항상 서브클래스에서 해당 타입 메서드를 오버라이딩하는 것을 **금지** 한다.

```swift
class Person {
  static func getName() {
    print("name")
  }
}
class PersonPerson: Person {
  override static func getName()  // cannot override static method
}
```

이렇게 static으로 선언된 타입 메서드 getName의 경우 서브클래스에서 오버라이드 하려는 경우 에러가 발생한다.


**2. class** : 오버라이딩을 허용

class는 서브클래스에서 타입 메서드 오버라이딩을 **허용** 한다.

```swift
class Person {
  class func getName() {
    print("name")
  }
}
class PersonPerson: Person {
  override class func getName()
}
```

이렇게 class로 선언된 타입메서드의 getName의 경우 서브클래스에서 오버라이딩을 해도 에러가 발생하지 않는다.




### 타입 메서드와 인스턴스 메서드의 멤버 접근 범위


- 일반 프로퍼티: 저장 프로퍼티 & 연산 프로퍼티
- 타입 프로퍼티: 저장 타입 프로퍼티 & 연산 타입 프로퍼티

```swift
class Person {
  let name = "zehye"  // 저장 프로퍼티
  static let alias = "zehyezehye"  // 저장 타입 프로퍼티
}
```

이렇게 name이라는 저장 프로퍼티와 alias라는 저장 타입 프로퍼티가 있을때 인스턴스 메서드와 타입 메서드 모두 이 둘을 사용할 수 있을까?

1. 타입메서드: 타입 멤버(프로퍼티&메서드)만 사용가능하고, 같은 타입멤버는 이름없이 접근 가능
2. 인스턴스 메서드: 인스턴스 멤버(프로퍼티&메서드)를 사용할 수 있고, 타입 멤버도 타입만 알면 접근 가능



#### 타입메서드의 경우

```swift
class Person {
  let name = "zehye"
  static let alias = "zehyezehye"

  static func sayHi() {
    print(name)  // error! Instance member 'name' cannot be used on type 'Person'
    print(alias)
  }
}
```

저장 프로퍼티인 name은 인스턴스멤버이다. <br>
따라서 인스턴스가 생성될때마다 메모리에 올라가는 인스턴스와 연관된 프로퍼티이다.

하지만 타입메서드는 인스턴스를 선언할 필요가 없는 메서드이기 때문에 자신과 같이 인스턴스를 생성할 필요없는 타입 프로퍼티의 경우, **같은 타입에 한해 타입 이름없이 접근이 가능** 하지만 인스턴스가 생성되어야만 저장공간을 갖는 인스턴스 멤버에는 접근이 불가능하다.



#### 인스턴스 메서드의 경우

```swift
class Person {
  let name = "zehye"
  static let alias = "zehyezehye"

  static func sayHi() {
    print(name)  
    print(Person.alias)
  }
}
```

반대로 인스턴스를 생성해야만 접근이 가능한 인스턴스 메서드에서는 인스턴스 멤버인 name에는 당연하게 접근이 가능하고(인스턴스 생성이 됐으니 해당 인스턴스 멤버인 name도 메모리에 올라감) 인스턴스와 상관없는 타입 멤버에 접근시 기존 방식과 같이 **타입 이름을 통해 접근이 가능** 하다.

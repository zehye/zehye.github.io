---
layout: post
title: Type Casting 다시 공부해보기!
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## 타입캐스팅이란?

타입 캐스팅은 인스턴스의 타입을 확인하거나, 해당 인스턴스를 슈퍼클래스나 하위 클래스로 취급하는 방법이다.<br>
swift에서 타입캐스팅은 is, as 연산자로 구현하며, 타입캐스팅을 사용해 타입이 프로토콜에 적합한지의 여부도 알 수 있다.



### is

타입을 체크하는 연산자.

런타입 시점에 실제 체크가 이루어지며 표현식이 타입과 동일하거나 타입의 서브클래스인 경우 true<br>
이 외의 경우 false 값을 반환한다. 즉 is 연산자는 타입을 체크하는 연산자로 반환값은 bool 형이다.

```swift
let name: String = "zehye"

name is String  // true
name is Int   // false
```

즉 내가 원하는 타입인지 확인할 때 사용할 수 있으며 동일한 타입인지 확인하기 위해서도 사용이 가능하다.<br>
그런데 더 나아가 표현식이 타입의 서브클래스인 경우에도 is 연산자를 통해 확인이 가능하다.


```swift
class Person {

}

class Teacher: Person {

}


let teacher: Teacher = .init()
teacher is Teacher  // true
teacher is Person   // true
```

이렇게 Person클래스를 상속받고 있는 Teacher클래스에서 생성된 teacher 인스턴스 또한 <br>
Person클래스의 서브클래스이기 때문에 Person으로 타입 체크를 해도 true를 반환한다.




### as

#### 업캐스팅 (Upcasting)

서브클래스 인스턴스를 슈퍼클래스의 타입으로 참조한다. <br>
업 캐스팅은 항상 성공한다. <br>
as 연산자를 사용해서 할수도 있다.

무슨말인가? 아래 예시를 살펴보자.

```swift
class Person {
  let name: String
  init(name: String) {
    self.name = name
  }
}

class Teacher: Person {

}

class Student: Person {

}

let people: [Person] = [
  Teacher.init(name: "박선생")
  Teacher.init(name: "김선생")
  Student.init(name: "이제자")
]
```

swift는 타입에 민감한 언어이기 때문에 위 예시처럼 people이라는 배열안에는 person이라는 타입의 인스턴스만 들어갈 수 있다.

그런데 Teacher, Student 타입의 인스턴스가 어떻게 들어가게 된걸까?<br>
이게 가능한 이유는 바로 **업캐스팅** 때문이다.

Teacher와 Student라는 클래스는 분명 명백히 서로 다른 타입의 클래스이다. <br>
이 둘의 유일한 공통점은 Person이라는 부모 클래스를 상속받고 있다는 것이다.

즉, 둘의 부모 클래스가 Person으로 동일하기 때문에 이 둘을 Person이란 클래스로 업캐스팅해서 묶어버린것을 의미한다.

아래 예시를 보자.

```swift
class Person {
  let name: String = "zehye"
}

class Teacher: Person {
  let subject: String = "English"
}

class Student: Person {
  let grade: Int = 1
}
```

요런 코드들이 있다고 할때

```swift
let human = Teacher.init() as Person
```

이 코드의 의미가 뭘까? 그건 바로<br>
**Teacher 타입의 인스턴스를 생성하지만, 이를 Person 타입으로 업캐스팅해서 human에 저장하겠다** 라는 의미이다.

실제 human의 타입은 업캐스팅 된 Person의 타입이다.<br>
즉, human이 Teacher란 서브클래스(Person이라는 슈퍼클래스 타입으로 참조하는) 업캐스팅을 한것으로<br>
human의 접근범위가 Person 멤버로 한정된다.

```swift
human.name  // zehye
human.subject  // Value of type "Person" has no member 'subject'
```

즉, Person 클래스 멤버인 name에는 접근 가능하지만 Teacher 멤버인 subject에는 접근이 불가능하다.<br>
따라서 이렇게 **서브클래스의 인스턴스를 슈퍼 클래스의 타입으로 참조하는 것** 을 업캐스팅이라고 한다.

업캐스팅은 항상 성공하기 때문에 <br>
(상속 관계에서 슈퍼 클래스의 멤버는 서브 클래스가 당연히 포함하기 때문에)<br>
as를 사용해도 되고 직접 타입을 명시해서 사용해도 된다.

**as** : 컴파일 시점에 타입 캐스팅(업캐스팅)을 하며 실패할 경우 에러가 발생한다.


#### 다운캐스팅(DownCasting)

슈퍼클래스 인스턴스를 서브 클래스 타입으로 참조한다.<br>
업 캐스팅된 인스턴스를 다시 원래 서브 클래스 타입으로 참조할 때 사용한다.<br>
다운 캐스팅은 실패할 수 있기때문에 as?, as! 연산자를 이용한다.

업캐스팅은 서브클래스의 인스턴스를 슈퍼클래스의 타입으로 참조하는것이었다면, 다운 캐스팅은 그 반대로 슈퍼클래스의 인스턴스를 서브 클래스의 타입으로 참조하는것을 의미한다. 그리고 이때 사용하는것이 타입캐스팅인 as? 흑은 as! 라는 연산자이다.

```swift
var teacher: Teacher = human as! Teacher
```

이렇게 as연산자 다음에 ? 혹은 !를 사용함으로써 Person타입으로 업캐스팅된 human 변수를 다시 하위 클래스인 Teacher 타입으로 변환해서 넣어주는것이 바로 다운캐스팅이다. 슈퍼클래스인 Person 타입의 인스턴스를 서브클래스인 Teacher 타입의 인스턴스로 참조한 것 !


따라서 아래와 같이 접근이 가능해진다.

```swift
teacher.subject  // English
```

그런데 문제는 다운캐스팅은 실패할 수가 있다.<br>
그렇기 때문에 실패할 가능성이 있는 다운캐스팅은 업캐스팅과는 달리 as에 옵셔널이 붙은 것이다.


#### as?

런타임 시점에 다운캐스팅을 하며, 실패할 경우 nil을 반환한다.<br>
컴파일 시점에는 성공/실패 여부를 알 수 없기 때문에 옵셔널 타입으로 선언을 해야한다.


#### as!

런타임 시점에 다운캐스팅을 하며, 실패할 경우 에러를 발생시킨다.<br>
따라서 가급적 as?를 사용하는 것이 안전한 방법이다.

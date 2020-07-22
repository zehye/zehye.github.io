---
layout: post
title: swift Hashable
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Hashable

> A type that provides an integer hash value.    
정수 Hash 값을 제공하는 타입의 프로토콜

- set 또는 dictionary의 key로 hashable을 준수하는 모든 타입을 사용할 수 있다.
  - swift에서 dictionary는 Dictionary<Key Type, Value Type> 형태로 쓰인다.
  - 이때 유일한 제약사항은 Key Type이 반드시 해시가능한 타입이어야 한다(Hashable)
  - 즉, **그 자체로 유일하게 표현이 가능한 방법을 제공해야한다는 뜻** 이다.
  - swift의 기본타입(String, Int, Double...)은 기본적으로 해시가능한 것들로 dictionary의 key type으로 사용가능하다.
  - 또한 swift의 enum(열거형) 또한 해시 가능하다.


표준 라이브러리의 많은 타입들 또한 hashable을 준수한다.

stirng, integer, floating-point, boolean values, set이 기본적으로 hash value를 제공하며(이때 set은 hashable한 것만 들어갈 수 있다) 자신만의 사용자 정의 타입도 hash가 가능할 수 있다. associated valuew없이 열거형(enum)을 정의하면, hashable 준수가 자동으로 적용되고, hash value 프로퍼티를 추가해 사용자 정의 타입에 hashable 준수를 추가할 수도 있다.

타입의 hash value 프로퍼티에 의해 제공되는 hash값은 동일하게 비교되는 임의의 두 인스턴스에 대해 동일한 정수이다.

즉, 같은 타입의 인스턴스인 a와 b의 경우 a==b이면 a.hashvalue == b.hashvalue가 된다는 것을 의미한다.<br>
그러나 반대의 경우에는 true가 아닐 수도 있다. 동일한 hash값을 가진 두개의 인스턴스가 서로 동일할 필요는 없기 때문이다.

**hash 값은 프로그램 실행에 따라 동일하지 않을 수 있다. 향후 실행에 사용할 hash값을 저장하지 않아야 한다**



### Conforming to the Hashable Protocol

set은 hashable한 것만 들어갈 수 있으며 dictionary의 key역시 hashable하다고 했다.<br>
사용자 정의 타입을 set에 넣거나 dictionary key로 만들고 싶다면 타입이 hashable을 준수하면 된다.

사용자 정의 타입의 hashable과 equatable 요구사항은 타입이 다음 조건을 충족시킬 때 컴파일러에서 자동으로 합성(synthesized)해준다.

- 구조체의 경우, 저장 프로퍼티는 모두 hashable을 준수해야 한다.
- 열거형의 경우, 모든 associated values은 모두 hashable을 준수해야 한다.
  - associated values가 없는 열거형은 정의없이(hash value) hashable을 준수한다.


타입의 hashable 준수를 커스터마이즈하거나, 위에서 말한 기준에 맞지 않는 형식으로 hashable을 채택하거나, 또는 이미 존재하는 타입을 hashable 준수하도록 확장하는 경우에는 사용자 지정 타입에 hashvalue 프로퍼티를 구현한다. 타입이 hashable 및 equatable 프로토콜의 의미론적 요구사항(semetic requirements)을 충족시키려면 타입의 equatable 준수를 사용자 정의하여 일치시키는 것이 좋다.


#### 예 - 버튼 그리드의 위치

버튼 그리드의 위치를 설명하는 GridPoint 타입을 보자. GridPoint의 초기선언은 아래와 같다.

```swift
struct GridPoint {
  var x: Int
  var y: Int
}
```

이제 여기서 사용자가 탭한 grid point의 set을 만들고 싶다고 가정해보자.

GridPoint타입은 아직 해시가능하지 않기에, set의 element 타입으로 사용할 수 없다.<br>
hashable을 준수하려면 == 연산자와 hashvalue 프로퍼티를 제공해야하기 때문이다.

```swift
extension GridPoint: Hashable {
  var hashValue: Int {
    return x.hashValue ^ y.hashValue &* 16777619
  }

  static func == (lhs: GridPoint, rhs: GridPoint) -> Bool {
    return lhs.x == rhs.x && lhs.y == rhs.y
  }
}
```

근데 이제 swift 4.1부터는 변경사항이 생겼다고 한다. hashable 하고싶으면 바로

```swift
struct GridPoint: Hashable {
  var x = Int
  var y = Int
}

var zehyeSet = Set<GridPoint>()
var zehyeDict: [GridPoint: Int] = [:]
```

즉 hashValue를 만들어주지 않았음에도 불구하고 컴파일러가 자동으로 만들어준 것이다. 이게 가능한 이유는

```swift
/// A point in an x-y coordinate system.
struct GridPoint: Hashable {
  var x = Int
  var y = Int
}
```

GridPoint타입의 저장프로퍼티가 모두 hashable한 값들이기 때문이다.(구조체의 경우~~~)

따라서 hashable 프로토콜을 사용자 정의 타입에서 채택만 하면 이제 set에 들어갈수도 dictionary의 key로도 들어갈 수 있음을 의미한다.



<hr>

### Set, Dictionary key가 되려면 왜 hashable 해야할까?


> 해시함수는 임의의 길이의 데이터를 고정된 길이의 데이터로 매핑하는 함수이다.      
이 용도중 하나로는 해시테이블이라는 자료구조에 사용되며 매우 빠른 데이터 검색을 위한 컴퓨터 소프트웨어에 널리 사용된다.      
해시 함수는 큰 파일에서 중복되는 레코드를 찾을 수 있기에 데이터베이스 검색이나 테이블 검색 속도를 가속할 수 있다.


즉, swift의 set과 dictionary key에는 순서가 없다. hashable의 정의가 **"정수 hash값을 제공하는 타입"** 인데 이 정수 hash가 있기 때문에 우리가 찾으려는 원소를 더 빨리 찾을 수 있게 되는 것이다.

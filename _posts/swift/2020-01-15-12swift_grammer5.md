---
layout: post
title: swift 기본문법 - 컬렉션 타입(Array, Dictionary, Set)
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
인프런, 야곰의 스위프트 기본문법 강좌를 듣고 정리하였습니다.

<hr>


## 컬렉션 타입

여러 값들을 묶어 하나의 변수를 표현할 수 있도록 해준다.


### Array

순서가 있는(인덱스가 있는) 리스트 컬렉션

```swift

var integers: Array<Int> = Array<Int>()  // []

integers.append(1)  // [1]
integers.appenr(100)  // [1,100]

integers.contains(100)  // true
integers.contains(99)  // false

// 0번 인덱스의 값을 삭제
integers.remove(at: 0)  // 1

// 마지막 인덱스 값을 삭제
integers.remove()  

// 모든 인덱스 값을 삭제
integers.removeAll()  

integers.count()

integers[0]
```

#### Array를 생성하는 다양한 방법

```swift
var doubles: Array<Double> = [Double]()

var strings: [String] = [String]()

var characters: [Character] = []

// let을 사용해 Array를 선언하면 불면 Array가 된다.
let immutableArray = [1,2,3]  
```

### Dictionary

키와 값의 쌍으로 이루어진 컬렉션(key, value)

특별한 정렬 순서는 정해져있지 않다.

```swift
var anyDictionary: Dictionary<Stinrg: Any> = [String: Any]()

anyDictionary["someKey"] = "value"
anyDictionary["anotherKey"] = 100

// key에 해당하는 value를 삭제하고 싶을 때
anyDictionary.removeValue(forKey: "anotherKey")
anyDictionary["someKey"] = nil

// let으로 선언했기 때문에 Dictionary의 값을 변경할 수 없다
let emptyDictionary: [String: Stinrg] = [:]
let initalizedDictionary: [String: String] = ["name": "zehye", "gender": "female"]
```



### Set

순서가 없고, 멤버가 유일한 컬렉션(중복된 값이 없다)

```swift
var integerSet: Set<Int> = Set<Int>()
integerSet.insert(1)
integerSet.insert(100)
integerSet.insert(99)
integerSet.insert(99)
integerSet.insert(99)

integerSet  // {100,99,1}

integerSet.contains(1)  // true
integerSet.contains(2)  // false

integerSet.remove(100)
integerSet.removeFirst()

integerSet.count  

let setA: Set<Int> = [1,2,3,4,5]
let setB: Set<Int> = [3,4,5,6,7]

// A와 B의 합집합
let union: Set<Int> = setA.union(setB)  // {2,4,5,6,1,3,7}

// 합집합을 정렬
let sortedUnion: [Int] = union.sorted()  // {1,2,3,4,5,6,7}

// 교집합
let intersection: Set<Int> = setA.intersection(setB)  // {5,3,4}

// 차집합
let subtracting: Set<Int> = setA.subtracting(setB)  // {2,1}
```

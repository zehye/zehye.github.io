---
layout: post
title: swift 기본문법 - 서브스크립트(Subscript)
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## 서브스크립트(Subscript)

컬렉션, 리스트, 시퀀스 타입의 개별 요소에 접근할 수 있는 지름길을 제공하는 것


```swift
let array = [1, 2, 3]
```

위와 같은 배열이 있을때 배열 내부의 2라는 값을 얻기 위해 array[1] 이라는 문법을 사용한다.

여기에서 array **[1]** 이 부분이 바로 서브스크립트이다. 즉, 원하는 값을 쉽게 찾기 위해 정의하는 문법을 서브스크립트라고 한다.<br>
array가 서브스크립트 문법을 구현하지도 않았는데 사용할 수 있는 이유는 스위프트 표준 라이브러리에 정의된 array 구조체 내부에 서브스크립트가 이미 구현되어 있기 때문이다.


### 구현해보기

- subscript 키워드를 사용
- 연산 프로퍼티에서 사용했던 get, set 키워드 사용


```swift
subscript(index: Int) -> Int {
    get {
        // return an appropriate subscript value here
    }
    set(newValue) {
        // perform a suitable setting action here
    }
```


#### 예제

1.구구단

```swift
struct TimesTable {
    let multiplier: Int
    subscript(index: Int) -> Int {
        return multiplier * index  // subscript에 입력한 정수만큼 곱하기, set없이 읽기전용
    }
}

let threeTimesTable = TimesTable(multiplier: 3)  // 구구단 3단
print("six times three is \(threeTimesTable[6])")  // 18
```


2.영화 리스트

```swift
class MovieList{
    private var tracks = ["s", "d", "r"]
    subscript(index: Int) -> String {
        get {
            return self.tracks[index]
        }
        set {
            self.tracks[index] = newValue
        }
    }
}
 var movieList = MovieList()
print("영화리스트에서 두번째 영화는: \(movieList[1])")  // d
```

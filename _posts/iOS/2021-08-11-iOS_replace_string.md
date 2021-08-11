---
layout: post
title: iOS replacingOccurrences 사용해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## replacingOccurrences

앱을 개발하다보면 문자열에서 특정 문자열을 제거 / 변경하여 사용해야하는 경우가 있다.<br>
이때 사용하는 메소드가 `replacingOccurrences`이다.

```swift
import Foundation

let string = "안-녕하세요"
string.replacingOccurrences(of: "-", with: "")
```

이렇게 사용하는것의 의미는 아래와 같다.<br>
string안에 들어간 "-"를 ""로 바꾸어 새로운 문자열로 반환해 달라.


```swift
func replacingOccurrences(of target: String, with replacement: String) -> String
```

즉, target 파라미터에 바꾸고 싶은 문자열을 전달하고, replacement 파라미터에 바꿀 문자열을 전달해주면 된다.

예제는 또 아래와 같다.

```swift
func hideName(myName: String) -> String {
    let secontIndex = myName[myName.index(after: myName.startIndex)]
    return myName.replacingOccurrences(of: String(secontIndex), with: "*")
}

hideName(myName: "홍길동")
// return : "홍*동"
```

단 한가지 이를 사용해서 아쉬운 점은 첫번째 예시에서 처럼 만약에 바꾸려는 상수 string이 `안-녕-하세요`라고 해보자.<br>
나는 첫번째 -만 대치해서 바꾸고 싶은건데 replacingOccurrences를 사용하게 되면 모든 -가 다 변경되고 만다.

이런때에는 replacingOccurrences이 아닌 다른 방법을 사용하는것이 좋다.<br>
이때의 이런때는 내가 명확하게 없애고 싶은 문자열이 딱 존재할때를 의미한다.


```swift
func hideName(myName: String) -> String {
    var myName = myName.map { String($0) }
    myName[1] = "*"
    return myName.joined()
}

hideName(myName: "베르나르 베르베르")
// return : "베*르나르 베르베르"
```

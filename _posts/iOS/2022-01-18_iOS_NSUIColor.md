---
layout: post
title: iOS Cannot assign value of type 'UIColor' to type '[NSUIColor]' (aka 'Array<UIColor>') 에러 해결하기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Cannot assign value of type 'UIColor' to type '[NSUIColor]' (aka 'Array<UIColor>')

컬러값을 어레이에 담아서 가져오고 있는데 위와 같은 에러가 발생했다.

받아오고 있는 컬러 데이터는 아래와 같다.<br>
`["#1301239", "#123ff1", "#131942"]` 이런식으로..!(안의 숫자는 그냥 임의로 적어넣은 것!)

이렇게 어레이에 담겨져 있는 스트링 값들을 UIColor로 변환하고 싶은데 어떻게 해야할까?

```swift
let color = ["#1301239", "#123ff1", "#131942"]
let hexColor = [NSUIColor(cgColor: UIColor(hexString: color).cgColor)]
```

이렇게 해주면 간단하게 완료우~!

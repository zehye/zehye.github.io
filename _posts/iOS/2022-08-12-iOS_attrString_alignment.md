---
layout: post
title: iOS Label Text를 가운데 정렬하기 
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Label Text를 가운데 정렬하기

이렇게 간단한걸 왜 굳이 정리하지? 싶겠지만.... 네 그렇습니다.<br>
일반적으로 텍스트를 가운데 정렬하기 위해서는 아래와 같이 코드를 작성하면 됩니다.

```swift
self.titleLbl.textAlignment = .center
```

그런데 우리가 보통 텍스트의 속성을 건들기 위해서 `NSMutableAttributedString` 이 친구를 사용하는 경우가 있습니다.<br>
이 친구를 사용하다보면 이상하게도 위에서 가운데 정렬을 하던 코드가 제대로 동작을 하지 않게 됩니다.

그때는 아래와 같은 방법을 사용해줍니다!

```swift
let attrString = NSMutableAttributedString(string: self.titleLbl.text ?? "")
let paragraphStyle = NSMutableParagraphStyle()
paragraphStyle.alignment = .center

attrString.addAttribute(.paragraphStyle, value: paragraphStyle, range: NSRange(location: 0, length: attrString.length))

self.titleLbl.attributedText = attrString
```
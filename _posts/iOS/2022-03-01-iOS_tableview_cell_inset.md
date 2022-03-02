---
layout: post
title: iOS Tableview Cell inset > Cell 사이의 간격 주는 방법
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Tableview Cell 사이의 간격 주는 방법

```swift
override func layoutSubviews() {
  super.layoutSubviews()
  contentView.frame = contentView.frame.inset(by: UIEdgeInsets(top: 0, left: 0, bottom: 0, right: 0))
}
```

위 코드에서 top, left, bottom, right를 내가 원하는 간격만큼 넣어주면 된다.

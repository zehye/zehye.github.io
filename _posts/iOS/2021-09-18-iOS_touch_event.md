---
layout: post
title: 버튼의 터치가 눌려있는 상태로 시작하려면?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


```swift
button.addTarget(self, action: #selector(btnTap(_:)), for: .touchUpInside)
button.sendActions(for: UIControl.Event.touchUpInside)
```

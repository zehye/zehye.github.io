---
layout: post
title: iOS TextField에 그림자 넣어주는 방법
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## TextField에 그림자 넣어주는 방법

```swift 
self.textField.backgroundColor = ColorTheme(background: .white)
self.textField.borderStyle = .none
self.textField.layer.cornerRadius = 12
self.textField.layer.shadowOpacity = 0.5
self.textField.layer.shadowRadius = 0.3
self.textField.layer.shadowOffset = CGSize(width: 0, height: 2)
self.textField.layer.shadowColor = ColorTheme(foreground: .black100).withAlphaComponent(0.05).cgColor
```
---
layout: post
title: iOS WKWebkit view에 round(corner radius) 넣어주기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## WKWebkit view에 round(corner radius) 넣어주기

webkit view에 corner radius 를 적용하려니 되지 않더군요?

기존코드는 아래와 같습니다.

```swift 
webView.layer.cornerRadius = 12
```

실제 코드가 작동하기 위해서 아래 코드를 추가해줍니다.

```swift 
webView.layer.masksToBounds = true 
```


---
layout: post
title: iOS The layer is using dynamic shadows which are expensive to render 경고 없애는 방법
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## The layer is using dynamic shadows which are expensive to render

뷰에 그림자를 만들고나니 아래와 같은 문구가 뜨더군요?!

```
The layer is using dynamic shadows which are expensive to render. If possible try setting 'shadowPath', or pre-rendering the shadow into an image and putting it under the layer.
```

현재 재 코드는 아래와 같습니다.

```swift 
self.containerView.layer.shadowColor = UIColor.black.cgColor
self.containerView.layer.shadowOffset = CGSize(width: 0.0, height: 20)
self.containerView.layer.shadowRadius = 1.0
self.containerView.layer.shadowOpacity = 0.5
```

그럼 위 경고문구는 무슨 뜻이었을까요?

렌더링 비용이 많이 드니까, shadowPath를 변경하는 문구로 **UIBesierPath** 를 사용하란 의미입니다.

```swift 
self.containerView.layer.shadowPath = UIBezierPath(roundRect: containerView.bounds, cornerRadius: containerView.layer.cornerRadius).cgPath
```

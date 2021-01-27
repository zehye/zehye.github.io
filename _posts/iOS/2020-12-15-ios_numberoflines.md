---
layout: post
title: swift) UILable의 자동 줄바꿈하는 방법?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## UILable 자동 줄바꿈?

기본적으로 UILable을 사용하면 아래 사진처럼 자동 줄바꿈이 안된다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/67.png" alt="" width="50%">
</figure>
</center>

틀이 정해져있는 텍스트라면 스토리보드에서 line의 수를 정해주면 되지만, 우리가 만약 채팅방을 만든다고 생각해보자.<br>
채팅을 주고받을때 주고받는 텍스트의 길이는 유동적일 것이다.

이 유동적인 텍스트(데이터)에 대한 줄바꿈이 자동으로 되도록 하기 위해서는 아래처럼 진행하면 된다.

```swift
@IBOutlet weak var chatLbl: UILabel!

self.chatLbl.numberOfLines = 0
```

그러면 아래처럼 적용이 된다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/68.png" alt="" width="50%">
</figure>
</center>

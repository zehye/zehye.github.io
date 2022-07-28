---
layout: post
title: iOS controller 이동시 tabbar 사라지게 하는 방법
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/33.png" alt="" width="80%">
</figure>
</center>

Hide Bottom Bar on Push 를 체크해주면 된다.<br>
push로 왔을 시 bottom영역을 hide 할것인지를 묻는 체크박스!

## 코드로 수정을 원한다면?

푸시로 화면이동을 하는경우

```swift 
let vc = mainVC.instance()
vc.hideBottomBarWhenPushed = true
self.navigationController?.pushViewController(vc, animated: true)
```

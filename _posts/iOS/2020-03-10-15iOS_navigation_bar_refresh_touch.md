---
layout: post
title: Navivation Item, Bar Item 사용해보기(실습)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Navivation Item, Bar Item 사용해보기

뷰 컨트롤러에 Navivation Item을 추가해주고 title을 사진첩으로 바꿔줘보고, Bar Buttom Item을 추가해보자!

<center>
<figure>
<img src="/assets/post-img/iOS/86.png" alt="" width="80%">
</figure>
</center>

해당 버튼 아이템의 스타일을 refresh로 설정해보고 해당 버튼에 대한 액션을 만들어준다.

```swift
@IBAction func touchUpRefreshBtn(_ sender: UIBarButtonItem) {
    self.tableView.reloadSections(IndexSet(0...0), with: .automatic)
}
```

정상적으로 동작하는 것을 볼 수 있을 것이다.

그리고 추가로 네비게이션 컨트롤러는 툴바를 가질 수 있다. 해당 툴바에도 바 버튼을 추가할 수 있다.

<center>
<figure>
<img src="/assets/post-img/iOS/87.png" alt="" width="80%">
</figure>
</center>

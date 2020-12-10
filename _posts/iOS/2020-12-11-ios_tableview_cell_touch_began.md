---
layout: post
title: tableview 혹은 scrollview에서 화면 터치이벤트 받는 방법?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## touchesbegan이 안된다!?

보통 키보드가 올라온 상태에서 화면을 누르면 키보드가 내려가게 하고 싶다면 touchesbegan으로 처리를 했을 것이다.

```swift
override func touchesbegan(_ touches: Set(UITouch), with event: UIEvent?) {
  // todo...
}
```

지금 내가 하고 있는 프로젝트는 테이블뷰로 이루어지고 있는데, 이 테이블뷰의 셀에 터치를 시도하게 되면 키보드가 내려가질 않는 현상이 발생한다. <br>
이 touchesbegan이 먹히지 않는 이유는 찾아보니 생각보다 간단했다.

테이블뷰나 스크롤뷰 같은 경우는 일반적으로 스크롤을 하기위해 기본적인 **1터치** 가 발생한다. <br>
그렇기 때문에 한번의 터치같은 경우는 그냥 무시가 되는 것 같다. 따라서 **touchesbegan** & **touchedMoved** 모두 호출이 되지 않는 것이다.

따라서 이러한 경우 직접 **GestureRecognizer** 를 추가하여 핸들러를 캐치하도록 만들어주어야 한다.

```swift
override func viewDidLoad() {
    super.viewDidLoad()

    // 핸들러 등록
    view.addGestureRecognizer(UITapGestureRecognizer(target: self, action: #selector(handleTap(sender:))))
}

// 터치가 발생할때 핸들러 캐치
@objc func handleTap(sender: UITapGestureRecognizer) {
    if sender.state == .ended {
        view.endEditing(true) // todo...
    }
    sender.cancelsTouchesInView = false
}
```

이렇게 핸들러를 등록해주면 tableview 혹은 scrollview에서 터치이벤트를 작동할 수 있게된다.

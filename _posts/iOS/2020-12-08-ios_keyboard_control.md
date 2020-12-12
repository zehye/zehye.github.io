---
layout: post
title: 채팅 textView(입력창, 전송버튼이 합쳐진 뷰)를 키보드와 붙여서 올리고 내리는 방법
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 내가 원하는 화면

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/65.png" alt="" width="50%">
</figure>
</center>

내가 원하는 화면은 위와 같다.

현재 만들고 있는 창은 채팅방이고, 채팅을 입력하는 textView와 전송 버튼이 담겨진 uiView가 textView를 터치했을때 키보드와 함께 붙어서 올라가고 바깥을 터치할때 내려가도록 만들고 싶었다.


### 내가 한 코드

```swift
@IBOutlet weak var sendViewBottomMargin: NSLayoutConstraint!


func initNotification() {
    // 키보드 올라올 때
    NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillShow(noti:)), name: UIResponder.keyboardWillShowNotification, object: nil)
    // 키보드 내려갈 때
    NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillHide(noti:)), name: UIResponder.keyboardWillHideNotification, object: nil)
}

// 키보드 올라오기
@objc func keyboardWillShow(noti: Notification) {
    let notiInfo = noti.userInfo!
    // 키보드 높이를 가져옴
    let keyboardFrame = notiInfo[UIResponder.keyboardFrameEndUserInfoKey] as! CGRect
    let height = keyboardFrame.size.height - self.view.safeAreaInsets.bottom
    sendViewBottomMargin.constant = height

    // 애니메이션 효과를 키보드 애니메이션 시간과 동일하게
    let animationDuration = notiInfo[UIResponder.keyboardAnimationDurationUserInfoKey] as! TimeInterval
    UIView.animate(withDuration: animationDuration) {
        self.view.layoutIfNeeded()
    }
}

// 키보드 내려가기   
@objc func keyboardWillHide(noti: Notification) {
    let notiInfo = noti.userInfo!
    let animationDuration = notiInfo[UIResponder.keyboardAnimationDurationUserInfoKey] as! TimeInterval
    self.sendViewBottomMargin.constant = 0

    // 애니메이션 효과를 키보드 애니메이션 시간과 동일하게
    UIView.animate(withDuration: animationDuration) {
        self.view.layoutIfNeeded()
    }
}
```

위의 sendViewBottomMargin이 걸려있는 부분은 아래 사진과 같다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/66.png" alt="" width="50%">
</figure>
</center>

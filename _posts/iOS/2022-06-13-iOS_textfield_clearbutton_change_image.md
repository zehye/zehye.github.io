---
layout: post
title: iOS TextField의 Clear Button 이미지 변경하는 방법 
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## TextField의 Clear Button 이미지 변경하는 방법 

```swift 
// 클리어 버튼 이미지 변경하는 코드
if let clearButton = textField.value(forKeyPath: "_clearButton") as? UIButton {
    clearButton.setImage(UIImage(named: "변경하고 싶은 이미지"), for: .normal)
}

// 텍스트필드에 글자를 쓰고 있을때 클리어 버튼이 나타나도록 한다
self.textField.clearButtonMode = UITextField.ViewMode.whileEditing
```

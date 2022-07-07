---
layout: post
title: iOS TextField에 Inset 주는 방법 
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## TextField에 Inset 주는 방법 

UITextField 에 대한 Extension을 만들어줍니다. <br>
저는 텍스트필드의 왼쪽 영역에 inset을 줘 볼 예정입니다.


```swift 
import Foundation
import UIKit

extension UITextField {
    func addLeftPadding() {
        // width값에 원하는 padding 값을 넣어줍니다. 
        let paddingView = UIView(frame: CGRect(x: 0, y: 0, width: 주고싶은 만큼 주세요, height: self.frame.height))
        self.leftView = paddingView
        self.leftViewMode = ViewMode.always
    }
}
```

텍스트 필드를 직접 쓰는 뷰컨으로 돌아와 해당 메소드를 적용해줍니다.

```swift 
func setTextFieldUI() {
    self.textField.addLeftPadding()
}
```

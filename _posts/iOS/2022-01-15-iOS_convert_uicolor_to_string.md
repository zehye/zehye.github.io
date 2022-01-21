---
layout: post
title: iOS Convert UIColor to String(+# 제거하기!)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Convert UIColor to String

서버에서 컬러값을 받아오고 있는데 `"#293949"`이런식으로 받아오고 있었다.<br>
즉 여기서 필요한건 해당 스트링에서 #을 제거하고 UIColor 값으로 바꿔줘야 하는 것!

UIColor를 extension해주자. 추가한 뒤 아래 코드 복붙<br>
해당 방법은 되게 다양할텐데 나는 현재 이렇게 써주고 있다.

```swift
import Foundation
import UIKit

extension UIColor {

  convenience init(hexString: String) {

      let hex = hexString.trimmingCharacters(in: CharacterSet.alphanumerics.inverted)

      var int = UInt64()
      Scanner(string: hex).scanHexInt64(&int)
      let a, r, g, b: UInt64

      switch hex.count {
      case 3:
          (a, r, g, b) = (255, (int >> 8) * 17, (int >> 4 & 0xF) * 17, (int & 0xF) * 17)
      case 6:
          (a, r, g, b) = (255, int >> 16, int >> 8 & 0xFF, int & 0xFF)
      case 8:
          (a, r, g, b) = (int >> 24, int >> 16 & 0xFF, int >> 8 & 0xFF, int & 0xFF)
      default:
          (a, r, g, b) = (255, 0, 0, 0)
      }

      self.init(red: CGFloat(r) / 255, green: CGFloat(g) / 255, blue: CGFloat(b) / 255, alpha: CGFloat(a) / 255)
  }
}
```

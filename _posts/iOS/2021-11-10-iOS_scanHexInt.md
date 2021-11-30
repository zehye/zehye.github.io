---
layout: post
title: iOS iOS13 Deprecated scanHexInt32 수정하기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

프로젝트의 최소 버전을 iOS13으로 올리면서 발견한 Deprecated가 있었다.

```
scanHexInt32 was deprecated in iOS13
```

이전코드는 아래와 같았다.

```swift
extension UIColor {

  convenience init(hex: String, alpha: CGFloat = 1.0) {
    var cString:String = hex.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines).uppercased()

    if (cString.hasPrefix("#")) {
        cString = String(cString[cString.index(cString.startIndex, offsetBy: 1)...])
    }

    if (cString.count != 6) {
        return UIColor.gray
    }

    var rgbValue:UInt32 = 0

    Scanner(string: cString).scanHexInt32(&rgbValue)  // 해당 부분에서 위와 같은 경고 발생

    return UIColor(
        red: CGFloat((rgbValue & 0xFF0000) >> 16) / 255.0,
        green: CGFloat((rgbValue & 0x00FF00) >> 8) / 255.0,
        blue: CGFloat(rgbValue & 0x0000FF) / 255.0,
        alpha: CGFloat(1.0)
    )
  }

}
```

이는 아래와 같이 수정해주면 된다.


```swift
convenience init(hex: String, alpha: CGFloat = 1.0) {
    var hexFormatted: String = hex.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines).uppercased()

    if hexFormatted.hasPrefix("#") {
        hexFormatted = String(hexFormatted.dropFirst())
    }

    assert(hexFormatted.count == 6, "Invalid hex code used.")

    var rgbValue: UInt64 = 0
    Scanner(string: hexFormatted).scanHexInt64(&rgbValue)

    self.init(red: CGFloat((rgbValue & 0xFF0000) >> 16) / 255.0,
              green: CGFloat((rgbValue & 0x00FF00) >> 8) / 255.0,
              blue: CGFloat(rgbValue & 0x0000FF) / 255.0,
              alpha: alpha)
}
```

이렇게 UInt64 및 scanHexInt64로 업데이트해주면 됨!

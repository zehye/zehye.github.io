---
layout: post
title: String Emoji를 Image로 변환하기(UIGraphicsImage)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 해야할 문제

프로젝트를 하는 도중 emoji를 imageView에 랜덤으로 받아줘야하는 테스크를 받았다.<br>
여러 emoji를 스트링값으로 배열을 받아 랜덤 처리하는 방식은 생각해냈지만, 이를 이미지로 그려주는 함수를 애플에서 제공해주는지는 몰랐다.

1. String타입의 이모지를 Image 타입으로 전환
2. 버튼을 누르면 해당 이미지를 랜덤으로 ImageView에 보여주기


### extension + string

```swift
func emojiToImg() -> UIImage? {
    let size = CGSize(width: 100, height: 100)  // 내가 원하는 이미지 사이즈
    UIGraphicsBeginImageContextWithOptions(size, false, 0)
    UIColor.clear.set()

    let rect = CGRect(origin: .zero, size: size)
    UIRectFill(CGRect(origin: .zero, size: size))
    (self as AnyObject).draw(in: rect, withAttributes: [.font: UIFont.systemFont(ofSize: 100)])
    let image = UIGraphicsGetImageFromCurrentImageContext()
    UIGraphicsEndImageContext()

    return image
}
```


### ViewController

```swift
class ViewController: UIViewController {
    var randomImgList = ["🐶", "🙊", "🍔", "🎨", "🎃", "👾", "🏓"]

    @IBAction func touchRandomBtn(_ sender: UIButton) {
    let randomIndex = Int(arc4random_uniform(UInt32(randomImgList.count)))
    imgView.image = "\(randomImgList[randomIndex])".emojiToImg()
  }
}
```

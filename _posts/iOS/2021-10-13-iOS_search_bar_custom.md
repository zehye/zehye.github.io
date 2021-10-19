---
layout: post
title: iOS SearchBar 이것저것 건들여보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## SearchBar


### 1. SearchBar Icon 색상 변경해보기

```swift
self.searchBar.searchTextField.leftView?.tintColor = .white
```

처음에 아이콘이 아니라 어디서 이미지 불러오는건줄 알고.. 한참을 찾았던....ㅎ

### 2. SearchBar BackgroundColor 변경해보기

```swift
self.searchBar.searchTextField.backgoundColor = .clear
```

### 3. SearchBar Placeholder FontSize, TextColor 변경해보기

```swift
let placeholder = "안녕하세요"

let attributedString = NSMutableAttributedString(string: placeholder, attributes: [
                NSAttributedString.Key.font: UIFont.systemFont(ofSize: 16),
                NSAttributedString.Key.foregroundColor: UIColor(white: 255/255, alpha: 1)
])
```


### 4. Searchbar TextColor 변경해보기

- 첫번째 방법

```swift
if let textField = self.searchBar.value(forkey: "searchField") as? UITextField {
  textField.textColor = .white
}
```

- 두번째 방법: 위 처럼 해도 안되는 경우

AppDelegate로 갑시다

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
          UITextField.appearance(whenContainedInInstancesOf: [UISearchBar.self]).defaultTextAttributes = [NSAttributedString.Key.foregroundColor: UIColor.white]

          return true
}
```


### 5. SearchBar내 TextField의 Clear Button의 색상 변경해보기

```swift
if #available(iOS 13.0, *) {
    self.searchBar.searchTextField.clearButtonMode = .always
    if let button = searchBar.searchTextField.value(forKey: "_clearButton") as? UIButton {
      let templateImage = button.imageView?.image?.withRenderingMode(.alwaysTemplate)
      button.setImage(templateImage, for: .normal)
      button.tintColor = .white
    }
} else if let textField = self.searchBar.value(forKey: "_searchField") as? UITextField {
    textField.clearButtonMode = .always

    if let button = searchBar.searchTextField.value(forKey: "_clearButton") as? UIButton {
      let templateImage = button.imageView?.image?.withRenderingMode(.alwaysTemplate)
      button.setImage(templateImage, for: .normal)
      button.tintColor = .white
  }
}
```

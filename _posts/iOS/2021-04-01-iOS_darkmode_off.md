---
layout: post
title: iOS 다크모드 해제하는 방법
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 다크모드 해제하는 방법

### 1. ViewController에서 override

아래 코드처럼 내가 원하는 모드를 지정해주면 된다.


```swift
class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        overrideUserInterfaceStyle = .light     //라이트모드
        //overrideUserInterfaceStyle = .dark    //다크모드
    }
}
```

### 2. Info.plist 에서 지정해주기

`Add Row > UIUserInterfaceStyle > Dark or Light 로 지정`

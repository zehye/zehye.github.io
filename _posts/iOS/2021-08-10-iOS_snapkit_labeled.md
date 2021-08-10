---
layout: post
title: Snapkit에서 labeled란?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## labeled

코드로든 스토리보드로든 이를 통해 레이아웃을 짤때 레이아웃이 맞지 않아 콘솔에 오류가 생기는 경우가 있다.

이미지의 주소라던지 각 객체의 주소가 `0x000ff280` 이런식으로 나와있기 때문에 이게 어떤 객체를 말하는건지 파악이 안되는 경우가 많다. 이런때에 보통 breakpoint를 잡거나 레이아웃 에러를 잡아주는 사이트?를 들어가서 파악을 할 수도 있지만 스냅킷에서는 좀 더 편리한 방법이 잇다.

그게 바로 **labeled** 기능이다.

```swift
titleLbl.snp.makeConstraint { make in
  make.width.equalToSuperView().multipliedBy(0.45).labeled("titleLbl")
}
```

이런식으로 labeled를 달아주면 이후에 콘솔창에 뭐가 문제인지 확인하기 편하진다.<br>
각 객체에 이름을 달아준다고 생각하면 쉬울 것 같다!

---
layout: post
title: iOS Facebook SDK 12.2.1 로 업그레이드하면서 수정할 것들
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## FBSDK 12.2.1 업그레이드

참고한 깃헙은 요것입니다 > [참고 Github](https://github.com/facebook/facebook-ios-sdk/blob/main/CHANGELOG.md)

FBSDK를 9.1.0 에서 12.2.1 로 업그레이드 하였습니다.<br>
코코아팟을 사용했구요.


```vim
pod 'FBSDKShareKit', '12.2.1'
pod 'FBSDKCoreKit', '12.2.1'
pod 'FBSDKLoginKit', '12.2.1'
```


### 1. Incorrect argument label in call (have 'fromViewController:content:delegate:', expected 'viewController:content:delegate:')

친절하게도 이 에러에서는 대체해야할 문구를 바로 알려주었다.

`Replace 'fromViewController' with 'viewController'`



### 2. Instance member 'activateApp' cannot be used on type 'AppEvents'; did you mean to use a value of this type instead?

```swift
AppEvents.activateApp()  // 구버전
AppEvents.shared.activateApp()  // 요렇게 바꿔주기
```


### 3. Extra argument 'completionHandler' in call

`GraphRequest.start(completionHandler:)` 요게 `GraphRequest.start(completion:)` 이렇게 바뀌었다고 한다.

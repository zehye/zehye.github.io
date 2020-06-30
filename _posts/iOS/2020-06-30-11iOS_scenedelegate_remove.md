---
layout: post
title: Scene Delegate 삭제하기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## SceneDelegate 삭제하기

xCode 11.2로 새 프로젝트를 만들면 SceneDelegate.swift파일이 추가 된다.<br>
이를 그냥 지우고 빌드하면 검은 화면이 나오고, iOS13이상으로 강제하여야 하기 때문에 제거하는 법을 정리해보려 한다.


### 1. Info.plist에서 Application Scene Manifest 삭제

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/36.png" alt="" width="50%">
</figure>
</center>


### 2. SceneDelegate.swift 파일 삭제

### 3. AppDelegate.swift 내 UISceneSession Lifecycle 함수 삭제

```
// MARK: UISceneSession Lifecycle

func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration {
    // Called when a new scene session is being created.
    // Use this method to select a configuration to create the new scene with.
    return UISceneConfiguration(name: "Default Configuration", sessionRole: connectingSceneSession.role)
}

func application(_ application: UIApplication, didDiscardSceneSessions sceneSessions: Set<UISceneSession>) {
    // Called when the user discards a scene session.
    // If any sessions were discarded while the application was not running, this will be called shortly after application:didFinishLaunchingWithOptions.
    // Use this method to release any resources that were specific to the discarded scenes, as they will not return.
}
```

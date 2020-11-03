---
layout: post
title: Remote push notification 시뮬레이터에서 실행해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Remote push notification

Xcode 11.4부터 시뮬레이터에서 [Remote Push Notification](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1)의 시뮬레이션을 지원한다. 시뮬레이션을 하는 방법은 두가지가 있다.

1. 시뮬레이터에 apns 파일을 직접 드래그
2. CLI를 이용


### 시작

Remote Push Notification을 시뮬레이션 하기 위해서는 우선 사용자에게 **푸시알림 사용 권한을 요청하고 허가를 받아야한다.** <br>
권한 요청을 위해 `AppDelegate`파일의 `didFinishLaunchingWithOptions` 델리게이트에 권한 요청 코드를 추가해줘야 한다.

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
   requestAuthorizationForRemotePushNotification()
   return true
}

// 사용자에게 푸시 권한을 요청
func requestAuthorizationForRemotePushNotification() {
    let current = UNUserNotificationCenter.current()
    current.requestAuthorization(options: [.alert, .sound, .badge]) { (granted, error) in
    	// granted가 true로 떨어지면 푸시를 받을 수 있다.
    }
}
```

위 코드 혹은 아래 코드를 작성해주면 된다. (둘 중 하나만)

```swift
let authOptions: UNAuthorizationOptions = [.alert, .badge, .sound] UNUserNotificationCenter.current().requestAuthorization(options: authOptions) { (isAllowed, error) in
  if isAllowed { DispatchQueue.main.async(execute: { application.registerForRemoteNotifications() })
  }
}
```

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/61.png" alt="" width="50%">
</figure>
</center>

코드 작성 후 앱을 실행하면 권한 요청 팝업이 뜨게 될텐데, 허용(Allow)을 선택해 Remote Push를 사용할 수 있도록 해준다.



### 1. 시뮬레이터에 apns 파일을 직접 드래그 하는 방법

apns 파일은 `json` 포맷의 확장자가 `apns`인 파일이다.<br>
apns의 payload 형식을 따라 샘플 파일을 생성한다.


```
{
    "Simulator Target Bundle" : "App Bundle ID",
    "aps" : {  // Remote push notification 사용을 위해 지정된 payload
        "alert" : {
            "title" : "테스트입니다.",
            "body" : "테스트 푸시~",
        },
    },
}
```
해당 파일을 내 프로젝트 폴더 안에 `test.apns`로 저장하였다. 그리고 시뮬레이터에 직접 드래그를 해보면 된다!


### 2. CLI를 사용해보는 방법

터미널에서도 시뮬레이션을 할 수 있다. <br>
시뮬레이션에는 `xcrun` 명령을 사용한다.

```
xcrun simctl push [사용 가능한 디바이스 identifier] [App Bundle ID] [APNS 파일명]
```

1. 가능한 디바이스 확인하기

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/63.png" alt="" width="80%">
</figure>
</center>

단순히 `xcrun simctl list devices`를 하게 되면 모든 시뮬레이터 기종이 나오게 되는데, 이때 시뮬레이터 상태가 `Booted`인 시뮬레이터 identifier를 골라주어야 한다. 아래 코드를 작성하면 현재 내가 실행중이고 사용가능한 시뮬레이터를 찾을 수 있다.

```
xcrun simctl list devices | grep Booted
```

그러고 아래의 코드를 작성해주자.

```
xcrun simctl push 169DEF1A-288A-46C8-846F-8EB6079FD00C [App Bundle ID] test.apns
```

그러면 정상적으로 작동하는 것을 볼 수 있을 것이다.


<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/64.png" alt="" width="80%">
</figure>
</center>

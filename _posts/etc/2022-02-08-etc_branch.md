---
layout: post
title: iOS Branch Library 사용해보기
category: etc
tags: [branch, matrix]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## 딥링크

상세 콘텐츠로 유저를 랜딩 시키는 링크<br>
대부분의 웹 링크가 딥링크에 속한다.

- 다이렉트 딥링크: 링크가 열리기 전에 앱이 이미 설치되어 있어야 유저를 앱 콘텐츠로 이동 가능
- 디퍼드 딥링크: 링크가 열리는 시점에 앱이 설치되어있지 않아도 유저가 앱 설치 후 처음 오픈했을 때 원하는 콘텐츠로 이동 가능


### 유니버설 링크

iOS9에 유니버설 링크 도입

유저는 이를 통해 웹 페이지 혹은 앱 내의 콘텐츠로 이동이 가능하다. 유저가 유니버설 링크를 클릭하면, iOS는 기기에 특정 도메인에 해당하는 앱이 설치되어있는지를 확인 > 앱이 설치되어 있는 경우 웹 페이지를 로드하지 않고 앱이 즉시 실행된다. 앱이 설치되지 않은 경우 웹 URL이 Safari에서 로드됨.


### 사용 장점

1. 유저가 원하는 정보 제시
2. 유저의 재방문 유도
3. 유저 이탈 방지
4. 구매 의향 높은 유저 확보


### 사용방법

#### AppDelegate

```swift
import Branch

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    setupBranchSDK(launchOptions: launchOptions)
}

func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey: Any] = [:]) -> Bool {
    Branch.getInstance().application(app, open: url, options: options)
    return true
}

func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([Any]?) -> Void) -> Bool {
    Branch.getInstance().continue(userActivity)
    return true
}

func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
    Branch.getInstance().handlePushNotification(userInfo)
}

private func setupBranchSDK(launchOptions: [UIApplication.LaunchOptionsKey: Any]?) {
    #if DEBUG
    Branch.getInstance().setDebug()
    #endif
    Branch.getInstance().initSession(launchOptions: launchOptions) { params, _ in
    if let params = params as? [String: AnyObject] {
        if let refId = params["reference_id"]?.integerValue {
            /// ******** handle deep linking here *********
        }
    }
}
```


#### WebSite

[Branch Site 바로가기](https://dashboard.branch.io/)


### 딥링크 처리 로직

- 딥링크 URL인지 확인
  - application(_: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool  위 함수에서 화면을 초기화 하는 로직이 있다면, 딥링크로 인해서 이미 화면이 존재하는지를 체크해야 이중으로 초기화가 안됨

- URL 확인
  - application(_: UIApplication, open: URL, options: [UIApplication.OpenURLOptionsKey: Any] = [:]) -> Bool  위 함수에서 deeplink URL인지 확인

- 특정 화면으로 이동하기 전에 로그인이 되어있는지 체크
- 특정 화면으로 이동하기 전에, home을 먼저 push후 이동
- 만약 현재 home화면이, navigationController.viewControllers[0]에 위치한다면, 따로 home화면 push하지 않아도 됨


#### 참고
[1](https://jinsangjin.tistory.com/146)
[2](https://ios-development.tistory.com/207)
[3](https://medium.com/@dimpler03/branch-deeplink-handling-in-ios-b9178dab693f)

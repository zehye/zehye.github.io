---
layout: post
title: iOS 카카오, 네이버, 페이스북 로그인 연동 시 발생했던 오류 해결 총 정리!
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
에러가 발생할 때마다 바로바로 업데이트할 예정입니다. :) 

<hr>


## 네이버 로그인


### 1. LSApplicationQueriesSchemes 설정 에러가 나는 경우

```vim
-canOpenURL: failed for URL: "naversearchapp://" - error: "This app is not allowed to query for scheme naversearchapp"
-canOpenURL: failed for URL: "naversearchthirdlogin://" - error: "This app is not allowed to query for scheme naversearchthirdlogin"
-canOpenURL: failed for URL: "naversearchapp://" - error: "This app is not allowed to query for scheme naversearchapp"
```

- info.plist에서 LSApplicationQueriesSchemes 설정을 해주었는지 확인
- naversearchapp, naversearchthirdlogin 두개 추가해주는 것 잊지말자!


### 2. 사용 테스트 기기에 네이버 앱이 없다고 나오는 경우

```vim
-canOpenURL: failed for URL: "naversearchapp://" - error: "The operation couldn’t be completed. (OSStatus error -10814.)"
-canOpenURL: failed for URL: "naversearchthirdlogin://" - error: "The operation couldn’t be completed. (OSStatus error -10814.)"
-canOpenURL: failed for URL: "naversearchapp://" - error: "The operation couldn’t be completed. (OSStatus error -10814.)"
-canOpenURL: failed for URL: "naversearchapp://" - error: "The operation couldn’t be completed. (OSStatus error -10814.)"
-canOpenURL: failed for URL: "naversearchapp://" - error: "The operation couldn’t be completed. (OSStatus error -10814.)"
+[NaverThirdPartyLoginConnection sizeWithText:withFont:]: unrecognized selector sent to class 0x1001bddd0
*** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '+[NaverThirdPartyLoginConnection sizeWithText:withFont:]: unrecognized selector sent to class 0x1001bddd0'
```

- 사용하는 테스트 기기에 네이버 앱이 깔려있는지 확인!


### 3. 인증절차 없이 그냥 들어가져 버리는 경우

```vim
SNSLogin[2337:1925839] Request could not be completed because Connection is Busy
```

- 인증절차 없이 그냥 들어가져버리는 현상이 발생
- 토큰을 지우고 다시 진행해보자


### 4. 로그인은 되는데 발생하는 시스템 오류가 나오는 경우

```vim
[connection] nw_read_request_report [C8] Receive failed with error "Software caused connection abort"
```

- 백그라운드 작업 중에 네트워크 요청이 종료될 때 발생하는 기본 시스템 오류
- 백그라운드에서 완료할 수 있을 만큼 충분히 시간 유지가 필요


## 카카오 로그인

[참고](https://developers.kakao.com/sdk/reference/ios/release/KakaoSDKCommon/Enums/ClientFailureReason.html#/s:14KakaoSDKCommon19ClientFailureReasonO7UnknownyA2CmF)

### 1. 사용 테스트 기기에 네이버 앱이 없다고 나오는 경우

```vim
-canOpenURL: failed for URL: "kakaokompassauth://authorize" - error: "The operation couldn’t be completed.
ClientFailed(reason: KakaoSDKCommon.ClientFailureReason.Cancelled, errorMessage: Optional("The KakaoTalk authentication has been canceled by user."))
```

- 사용하는 테스트 기기에 카카오톡 앱이 깔려있는지 확인!



## 페이스북 로그인

### 1. 페이스북 app_id를 찾을 수 없다고 나오는 경우

```vim
*** Terminating app due to uncaught exception 'InvalidOperationException', reason: 'App ID not found. Add a string value with your app ID for the key FacebookAppID to the Info.plist or call [FBSDKSettings setAppID:].'
```

- AppDelegate, func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool 함수 내에 `FBSDKCoreKit.Settings.appID` 추가



### 2. 권한 접근이 불가능하다고 나오는 경우

```vim
facebook login is currently unavailable for this app, Since we ard updating additional details for this app.
```

- 페이스북 앱 개발자 콘솔로 들어간다.
- 앱 검토 > 권한 및 기능으로 이동 > public_profile 및 이메일을 고급 액세스 권한으로 설정

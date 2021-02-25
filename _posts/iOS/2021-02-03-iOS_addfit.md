---
layout: post
title: iOS 카카오 ADFit 연동해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

너무나 오랜만에 블로그를 작성하네요. 반성합니다.<br>
이번에 사이드 프로젝트를 진행하면서 앱 안에 광고를 넣으려면서 카카오에서 제공해주는 Adfit을 사용해보았는데..<br>
생각만큼 참고할만한 자료가 많이 없어서 고생고생하다가 고생기를 정리해보려고 합니다.

우선 참고하며 진행했던 자료는 요것입니다 >> [adfit-ios-sdk](https://github.com/adfit/adfit-ios-sdk/wiki/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)


## 시작해보기

### 지원 환경

1. 최신 버전의 Xcode (Xcode 12.0 / Swift 5.3)
2. iOS Deployment Target: iOS 12.0 이상

AdFitSDK는 Swift로 개발되어있으며, swift 기반의 프로젝트에서 AdFitSDK를 사용하시려면 반드시 최신 버전의 Xcode를 사용해주어야 합니다.


### 1. 광고단위 ID(Client ID) 발급받기

실제 광고를 수신하고 수익을 창출하기 위해서는, 먼저 AdFit 플랫폼에서 자신의 앱을 매체로 등록하고 광고단위 ID(Client ID)를 발급받아야 합니다.<br.
아래의 웹 사이트에서 앱 등록 / 광고단위 ID 발급 단계를 진행할 수 있습니다.

> [광고단위 ID 발급받기](http://adfit.kakao.com)

앱 등록과 광고단위 ID 발급이 완료된 후, 다음 단계의 안내에 따라 AdFit SDK를 설치한다.


### 2. SDK 설치하기

`pod 'AdFitSDK'` > `pod install`


### 3. 프로젝트 설정

#### ATS(App Transport Security)처리

iOS 9부터 ATS(App Transport Security) 기능이 기본적으로 활성화 되어 있기 때문에 암호화된 HTTPS 방식의 통신만 허용됩니다. AdFit SDK는 ATS 활성화 상태에서도 정상적으로 동작하도록 구현되어 있으나, **광고를 통해 노출되는 광고주 페이지는 HTTPS 방식을 지원하지 않을 수도 있기 때문** 에 아래의 사항을 앱 프로젝트의 **Info.plist** 파일에 적용하여 주어야 한다.


```
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```
<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/10.png" alt="" width="50%">
</figure>
</center>


#### ATT(App Tracking Transparency) framework 적용

iOS14 타겟팅된 앱이 IDFA 식별자를 얻기 위해서는 ATT Framework를 반드시 적용해야 한다.

```
<key> NSUserTrackingUsageDescription </key>
<string> 맞춤형 광고 제공을 위해 사용자의 데이터가 사용됩니다. </string>
```

이는 앱이 사용자 또는 장치를 추적하기 위해 데이터 권한을 요청하는 이유를 사용자에게 알리는 메세지입니다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/9.png" alt="" width="50%">
</figure>
</center>

이후 ATT 아래 코드를 App delegate에 적용시켜줍니다.

이는 광고 요청하기 전에 사용자로 부터 개인정보 보호에 관한 권한을 요청하는 것으로<br>
앱이 설치되고 한번만 호출하면 되며, 아래 코드는 사용자가 권한에 대한 응답 후에는 더 이상 사용자에게 권한을 묻지 않습니다.

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
  self.adFit()
}

extension AppDelegate {
    func adFit() {
        if #available(iOS 14, *) {
          ATTrackingManager.requestTrackingAuthorization(completionHandler: { status in
            // 권한 요청이 완료된 다음, 광고를 요청해 주세요.
//             loadAd()
          })
        }
    }
}
```

### 4. 광고 요청하기

```swift
import UIKit
import AdFitSDK

class MyViewController: UIViewController {
  override func viewDidLoad() {
      super.viewDidLoad()
      setAdfit()
  }

  func setAdfit() {
      let adView = AdFitBannerAdView(clientId: "", adUnitSize: "")
      adView.frame = CGRect(x: 0, y: 0, width: view.bounds.width, height: 100)
      adView.rootViewController = self
      adView.addSubview(adView)

      adView.loadAd()
      adView.delegate = self
  }
}
```


### 5. delegate 메서드 구현

```swift
func adViewDidClickAd(_ bannerAdView: AdFitBannerAdView) {
    print("adDidClicked")
}

func adViewDidReceiveAd(_ bannerAdView: AdFitBannerAdView) {
    print("adDidReceived")
}

func adViewDidFailToReceiveAd(_ bannerAdView: AdFitBannerAdView, error: Error) {
    print("adDidFailReceived - error :\(error.localizedDescription)")
}
```

하면 위 setAdfit함수에 적여있는 `adView.delegate = self`에 의해 광고가 잘 전달되었는지, 에러가 왜 발생하는지를 알 수 있게 됩니다.



### 발생했던 에러

```
The connection to service on pid 0 named com.apple.commcenter.coretelephony.xpc was invalidated from this process.
```

> xcrun simctl spawn booted log config --mode "level:off"  --subsystem com.apple.CoreTelephony

터미널에 위와 같이 실행해서 처리하였다.

[참고한 stackoverflow](https://stackoverflow.com/questions/52455652/xcode-10-seems-to-break-com-apple-commcenter-coretelephony-xpc)

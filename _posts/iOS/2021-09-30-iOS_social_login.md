---
layout: post
title: 소셜로그인(카카오, 네이버, 페이스북)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

- [Facebook](#1facebook)
- [Kakao](#2kakao)
- [Naver](#3naver)
- [Error Solution](#4error-solution)

## 1.Facebook

[Facebook developers 바로가기](developers.facebook.com/docs/facebook-login/ios)

### 터미널에서 pod 'FBSDKLoginKit' install 실행하기

```vim
pod 'FBSDKLoginKit'
```


### Facebook 개발자 사이트에서 앱 등록하기

#### 1. 앱만들기 탭 선택
#### 2. 앱만들기 유형 선택 후 표시할 이름 작성 & 이메일 작성해야 앱 만들기 버튼 활성화 됨

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/F1.png" alt="" width="50%">
</figure>
</center>


#### 3. Facebook 로그인 셀 선택

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/F2.png" alt="" width="50%">
</figure>
</center>


#### 4. iOS 셀 선택

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/F3.png" alt="" width="50%">
</figure>
</center>


### 개발 환경 설정은 아래 순서대로 합니다.

#### 1. 개발 환경 설정: pod 'FBSDKLoginKit' 했으면 패스

#### 2. 번들 ID(Xcode에서 설정해준 번들 아이디) 작성

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/F4.png" alt="" width="50%">
</figure>
</center>

#### 3. 앱에 대한 SSO 활성화 > 활성화로 선택

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/F5.png" alt="" width="50%">
</figure>
</center>


#### 4. info.plist 구성

```swift
<key>CFBundleURLTypes</key>
<array>
  <dict>
  <key>CFBundleURLSchemes</key>
  <array>
    <string>fb{APP-ID}</string>  // APP-ID 앞에 fb 붙는거 조심!
  </array>
  </dict>
</array>
<key>FacebookAppID</key>
<string>APP-ID</string>  // APP-ID 작성
<key>FacebookDisplayName</key>
<string>APP-NAME</string>  // 앱 생성할때 직접 만들어준 APP-NAME
```

```swift
<key>LSApplicationQueriesSchemes</key>
<array>
	<string>fbapi</string>
	<string>fbapi20130214</string>
	<string>fbapi20130410</string>
	<string>fbapi20130702</string>
	<string>fbapi20131010</string>
	<string>fbapi20131219</string>
	<string>fbapi20140410</string>
	<string>fbapi20140116</string>
	<string>fbapi20150313</string>
	<string>fbapi20150629</string>
	<string>fbapi20160328</string>
	<string>fbauth</string>
	<string>fb-messenger-share-api</string>
	<string>fbauth2</string>
	<string>fbshareextension</string>
</array>
```

#### 5. AppDelegate, SceneDelegate 설정

AppDelegate는 앱의 라이프사이클을 관리하는 부분으로 앱이 실행될 때마다 해주어야 하는 작업을 작성해준다. <br>
네이티브로 페이스북 앱을 열기 위해서는 앱을 실행할 때마다 SDK를 초기화해 주어야 한다.

```swift
import FBSDKCoreKit

...

// 앱이 실행된 후 호출되는 함수
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

       ApplicationDelegate.shared.application(application, didFinishLaunchingWithOptions: launchOptions)

       return true
}
```

iOS 13 이후부터는 SceneDelegate가 생기면서 내가 사용하는 버전이 iOS 13이후면 아래 코드는 SceneDelegate에 작성해주고 그렇지않다면 AppDelegate에 작성해준다. 나는 스토리보드를 사용하지 않고 코드로 프로젝트를 진행중이기 때문에 SceneDelegate가 없으므로 AppDelegate에 작성해주었다.

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
     ApplicationDelegate.shared.application(
        app,
        open: url,
        sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String,
        annotation: options[UIApplication.OpenURLOptionsKey.annotation]
    )
}
```

SceneDelegate가 있다면 아래 코드를 SceneDelegate에 작성해주도록 한다.

```swift
import FBSDKCoreKit

...

func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
    guard let url = URLContexts.first?.url else {
        return
    }

    ApplicationDelegate.shared.application(
        UIApplication.shared,
        open: url,
        sourceApplication: nil,
        annotation: [UIApplication.OpenURLOptionsKey.annotation]
    )
}
```

### ViewController에 페이스북 버튼 추가하기

페이스북 버튼을 추가하는 방법엔 페이스북 자체에서 제공해주는 버튼을 상속받아와 사용하는 방법과 직접 버튼을 구현해 함수를 연결해주는 방법이 있다.

#### 1. 페이스북 버튼 상속받아와 사용해보기

```swift
import FBSDKLoginKit
import SnapKit


...

class ViewController: UIViewController {

    override func viewDidLoad() {
      super.viewDidLoad()

      initView()
    }

    private let facebookBtn = FBLoginButton()

    private func initView() {
        self.view.addSubview(self.facebookBtn)

        self.facebook.snp.makeConstraint {(make) in
            make.top.equalToSuperView().inset(150)
            make.leading.trailing.equalToSuperView().inset(80)
        }
    }
}
```


#### 2. 커스텀 버튼 만들기

```swift
import FBSDKLoginKit
import SnapKit


...

class ViewController: UIViewController {

    override func viewDidLoad() {
      super.viewDidLoad()

      initView()
    }

    private let facebookBtn: UIButton = {
        let btn = UIButton()
        btn.backgroundColor = .blue
        btn.setTitle("페이스북 로그인", for: UIControl.State.normal)
        btn.setTitleColor(.white, for: UIControl.State.normal)
        return btn
    }()

    private let facebookLogoutBtn: UIButton = {
        let btn = UIButton()
        btn.setTitle("페이스북 로그아웃", for: UIControl.State.normal)
        btn.setTitleColor(.white, for: UIControl.State.normal)
        return btn
    }()

    private func initView() {
        self.view.addSubview(self.facebookBtn)
        self.view.addSubview(self.facebookLogoutBtn)

        self.facebook.snp.makeConstraint {(make) in
            make.top.equalToSuperView().inset(150)
            make.leading.trailing.equalToSuperView().inset(80)
        }

        self.facebookLogoutBtn.snp.makeConstraints {(make) in
            make.top..equalToSuperView().inset(80)
            make.trailing.equalToSuperView().inset(30)
        }

        self.facebookBtn.addTarget(self, action: #selector(self.facebookLogin(_:)), for: .touchUpInside)
        self.facebookLogoutBtn.addTarget(self, action: #selector(self.facebookLogout(_:)), for: .touchUpInside)
    }
}

extension ViewController {
    @objc func facebookLogin(_ sender: Any) {
        let loginManager = LoginManager()

        loginManager.logIn(permissions: ["public_profile", "email"], from: self) {(result, error) in
            guard error == nil else {
                print(error!.localizedDescription)
                return
            }

            guard let result = result, !result.isCancelled else {
                print("User cancelled login")
                return
            }

            GraphRequest.init(graphPath: "me", parameters: ["fields": "id, name, email, picture"])
            .start(completion: {(connection, result, error) -> Void in
                print("error: ", error ?? "No error")
                guard let fb = result as? [String: AnyObject] else { return }

                let token = fb["id"] as? String
                let name = fb["name"] as? String
                let email = fb["email"] as? String
                var profile = ""
                if let profileImg = fb["picture"] as? [String: AnyObject],
                let data = profileImg["data"] as? [String: AnyObject] {
                    profile = data["url"] as? String ?? ""
                }

                print("token: ", token ?? "no token")
                print("name: ", name ?? "no name")
                print("email: ", email ?? "no email")
                print("prfile_image: ", profile)
            })
        }
    }

    @objc func facebookLogout(_ sender: Any) {
        let loginManager = LoginManager()
        loginManager.logOut()
        print("facebook logout")

    }
}
```

<hr>

### 개발모드 > 라이브모드 설정해주기!

이렇게만 하면 실제 로그인이 이루어지지는 않는다. 그 이유는 현재 페이스북 로그인이 개발모드이기 때문이다.<br>
정상적으로 작동하기 위해서는 "라이브모드"로 전환해주어야 하는데, 저 스위치를 켜주게 되면 **개인정보처리방침** 과 **데이터 삭제 정보** 를 제공해주어야 한다는 메시지가 뜬다. 각 필드에 URL을 채워주면 라이브모드로 전환이 가능해진다.  

개인정보 처리방침 URL은 노션이나 블로그 게시글을 활용해 링크를 달아주어도 무관!

사용자 데이터 삭제 부분은 사용자가 삭제를 원할때 서버에서 모든 정보를 지울 수 있는 방법을 마련해 놓으라는 것으로 데이터 삭제 콜백 URL과 데이터 삭제 안내 URL 중 선택할 수 있다. 이렇게 모두 완료하면 비로소 라이브모드를 켤수 있게 되고, 로그인도 가능해진다.



## 2.Kakao

[Kakao developers 바로가기](https://developers.kakao.com/)

### Kakao 개발자 사이트에서 앱 등록하기

[홈페이지 '내 어플리케이션' 클릭] > [내 어플리케이션 추가하기 클릭] > [앱 이름 작성 후 저장]

#### 1. 플랫폼 등록

[만들어 놓은 앱 선택] > [플랫폼 설정하기 클릭] > [iOS 플랫폼 등록 클릭] > [번들 ID 작성 후 저장]


#### 2. 카카오 로그인 설정

[왼쪽 탭에서 카카오 로그인 클릭] > [활성화 설정 ON] > [활성화 버튼 클릭]

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/k1.png" alt="" width="50%">
</figure>
</center>

#### 3. 동의항목 설정

[왼쪽 탭에서 동의항목 클릭] > [원하는 항목 설정 버튼 클릭] > [필수/선택 동의 선택 후 동의목적 필수 작성, 카카오 계정으로 정보수집 후 제공 체크박스 선택]

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/K2.png" alt="" width="50%">
</figure>
</center>

- 프로필 정보를 제외한 다른 동의 항목의 '필수 동의'설정은 사용이 불가능함
- 설정한 동의 항목 내역은 내가 만든 앱의 카카오 로그인 동의 화면에 반영됨
- 동의 목적은 참고 문구로 카카오 로그인 동의 화면에는 나타나지 않으나, 해당 동의 하ㅇ목 이용 권한 검수에 사용됨


### KakaoSDK 설치하기

터미널에서 vi Podfile을 통해 SDK 작성해주고 pod install 까지 완료한다.

```vim
pod 'KakaoSDKCommon', '= 2.0.5'
pod 'KakaoSDKAuth', '= 2.0.5'
pod 'KakaoSDKUser', '= 2.0.5'
```

이렇게 iOS SDK를 설치하면 SDK에 필요한 외부라이브러리가 자동으로 설치된다 > Alamofire, DynamicCodable

### info.plist 설정하기

```swift
<key>LSApplicationQueriesSchemes</key>
  <array>
      <!-- 카카오톡으로 로그인 -->
      <string>kakaokompassauth</string>
      <!-- 카카오링크 -->
      <string>kakaolink</string>
  </array>
```


#### URL Scheme 설정하기

앱 > 타겟 > info에서 URL Type + 버튼 클릭

- URL Schemes 항목에 네이티브 앱 키를 `kakao{네이티브 앱 키}` 형식으로 등록해준다.
- 네이티브 앱 키는 좀전에 카카오 개발자 홈페이지에서 내가 등록한 어플리케이션을 선택하면 확인할 수 있다.
- 이 설정을 누락한다면 카카오링크 메시지를 통해 앱을 실행하는 것이 불가능해진다.


### AppDelegate에 KakaoSDK 초기화

```swift
import KakaoSDKCommon

...


func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
...
  KakaoSDKCommon.initSDK(appKey: "NATIVE_APP_KEY")
...
}
```

여기서도 iOS13 이하 혹은 SceneDelegate가 기본이 아닐때 추가해주는 함수가 존재한다.

```swift
import KakaoSDKAuth

...


class AppDelegate: UIResponder, UIWindowSceneDelegate {
   ...
   func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
    if (AuthApi.isKakaoTalkLoginUrl(url)) {
      return AuthController.handleOpenUrl(url: url)
    }
   return false
   }
   ...
}
```

SceneDelegate가 존재한다면 SceneDelegate에 아래 코드를 추가합니다.

```swift
import KakaoSDKAuth
...
class SceneDelegate: UIResponder, UIWindowSceneDelegate {
    ...
    func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
        if let url = URLContexts.first?.url {
            if (AuthApi.isKakaoTalkLoginUrl(url)) {
                _ = AuthController.handleOpenUrl(url: url)
            }
        }
    }
    ...
}
```

### ViewController에 카카오톡 버튼 추가하기

```swift
import KakaoSDKAuth

class ViewController: UIViewController {

    override func viewDidLoad() {
      super.viewDidLoad()

      initView()
    }

    private let kakaoBtn: UIButton = {
        let btn = UIButton()
        btn.backgroundColor = .yellow
        btn.setTitle("카카오톡 로그인", for: UIControl.State.normal)
        btn.setTitleColor(.white, for: UIControl.State.normal)
        return btn
    }()

    private let kakaoLogoutBtn: UIButton = {
        let btn = UIButton()
        btn.setTitle("카카오톡 로그아웃", for: UIControl.State.normal)
        btn.setTitleColor(.white, for: UIControl.State.normal)
        return btn
    }()

    private func initView() {
        self.view.addSubview(self.kakaoBtn)
        self.view.addSubview(self.kakaoLogoutBtn)

        self.kakaoBtn.snp.makeConstraint {(make) in
            make.top.equalToSuperView().inset(150)
            make.leading.trailing.equalToSuperView().inset(80)
        }

        self.kakaoLogoutBtn.snp.makeConstraints {(make) in
            make.top..equalToSuperView().inset(80)
            make.trailing.equalToSuperView().inset(30)
        }

        self.kakaoBtn.addTarget(self, action: #selector(self.kakaoLogin(_:)), for: .touchUpInside)
        self.kakaoLogoutBtn.addTarget(self, action: #selector(self.kakaoLogoutBtn(_:)), for: .touchUpInside)
    }
}

extension ViewController {
    @objc func kakaoLogin(_ sender: Any) {
        AuthApi.shared.loginWithKakaoTalk {(oauthToken, error) in
            if let error = error {
                print(error)
            } else {
                print("login kakao")
                _ = oauthToken
            }
        }
    }

    private func setUserInfo() {
        UserApi.shared.me {(user, error) in
            if let error = error {
                print(error)
            } else {
                print("me() success")
                _ = user
                print("nickname: \(user?.kakaoAccount?.profile?.nickname ?? "no nickname")")
                print("image: \(user?.kakaoAccount?.profile?.profileImageUrl)")
            }
        }
    }

    @objc func kakaoLogoutBtn(_ sender: Any) {
    UserApi.shared.logout{(error) in
        if let error = error {
            print(error)
        } else {
            print("kakao logout success")
        }
    }
}
```

## 3.Naver

[Naver developers 바로가기](https://developers.naver.com/products/login/api/)

### 어플리케이션 등록하기

#### 1. 상단 Application 탭에서 어플리케이션 등록 선택

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/N1.png" alt="" width="50%">
</figure>
</center>

#### 2. 어플리케이션 이름과 사용 API > 네아로 선택 후 저장

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/N2.png" alt="" width="50%">
</figure>
</center>

#### 3. 네아로 선택 시 제공 정보 선택 필수, 선택 체크박스 지정!

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/N3.png" alt="" width="50%">
</figure>
</center>


#### 4. 이때 로그인 오픈 API 서비스 환경 > iOS로 선택

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/N4.png" alt="" width="50%">
</figure>
</center>

- 다운로드 URL: 앱이 스토어에 올라가 있을 경우 그 URL을 작성하면 되며, 앱스토어에 등록되어 있지 않다면 개발 페이지 등의 사이트 주소를 작성
- URL Scheme: URL Scheme의 경우 프로젝트의 URL Types로 등록을 해야 한다.
  - 주의할 점은 `반드시 소문자로 작성`해야한다는 점이다.

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/N5.png" alt="" width="50%">
</figure>
</center>


### NaverSDK 설치하기

터미널에서 vi Podfile을 통해 SDK 작성해주고 pod install 까지 완료한다.

```vim
 pod 'naveridlogin-sdk-ios'
```

### info.plist 설정하기

```swift
<key>LSApplicationQueriesSchemes</key>
  <array>
    <string>naversearchapp</string>
    <string>naversearchthirdlogin</string>
  </array>
```


### AppDelegate에 NaverSDK 초기화

```swift
import NaverThirdPartyLogin

class AppDelegate: UIResponder, UIApplicationDelegate {

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        NaverThirdPartyLoginConnection.getSharedInstance().isInAppOauthEnable = true
        NaverThirdPartyLoginConnection.getSharedInstance().isNaverAppOauthEnable = true

        let instance = NaverThirdPartyLoginConnection.getSharedInstance()
        instance?.isNaverAppOauthEnable = true
        instance?.isInAppOauthEnable = true

        instance?.serviceUrlScheme = kServiceAppUrlScheme
        instance?.consumerKey = kConsumerKey
        instance?.consumerSecret = kConsumerSecret
        instance?.appName = kServiceAppName

        return true
    }
}
```

- kServiceAppUrlScheme: 애플리케이션 등록할 때 입력한 URL Scheme
- kConsumerKey: 애플리케이션 등록 후 발급받은 클라이언트 아이디
- kConsumerSecret: 애플리케이션 등록 후 발급받은 클라이언트 시크릿
- kServiceAppName: 애플리케이션 이름

위 설정을 위해서 `NaverThirdPartyConstantForApp.h` 파일로 이동해주세요. <br>
폴더 경로는 아래와 같습니다.

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/N6.png" alt="" width="50%">
</figure>
</center>

그리고 해당 파일에서 값들을 지정해줍니다.

<center>
<figure>
<img src="/assets/post-img/iOS/Social-Login/N7.png" alt="" width="50%">
</figure>
</center>

아래는 iOS 13 이하 버전 혹은 SceneDelegate가 기본이 아닌경우 AppDelegate에 추가할 코드입니다.

```swift     
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
      NaverThirdPartyLoginConnection.getSharedInstance()?.application(app, open: url, options: options)
      return true
}
```

SceneDelegate가 기본이라면 SceneDelegate에 아래 코드를 추가해줍니다.

```swift
func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
  NaverThirdPartyLoginConnection
    .getSharedInstance()?
    .receiveAccessToken(URLContexts.first?.url)
}
```

### ViewController에 네이버 버튼 추가하기

```swift
import NaverThirdPartyLogin

class ViewController: UIViewController {
  override func viewDidLoad() {
    super.viewDidLoad()

    initView()
  }

  private let naverBtn: UIButton = {
      let btn = UIButton()
      btn.backgroundColor = .yellow
      btn.setTitle("네이버 로그인", for: UIControl.State.normal)
      btn.setTitleColor(.white, for: UIControl.State.normal)
      return btn
  }()

  private let naverLogoutBtn: UIButton = {
      let btn = UIButton()
      btn.setTitle("네이버 로그아웃", for: UIControl.State.normal)
      btn.setTitleColor(.white, for: UIControl.State.normal)
      return btn
  }()

  private func initView() {
      self.view.addSubview(self.naverBtn)
      self.view.addSubview(self.naverLogoutBtn)

      self.naverBtn.snp.makeConstraint {(make) in
          make.top.equalToSuperView().inset(150)
          make.leading.trailing.equalToSuperView().inset(80)
      }

      self.naverLogoutBtn.snp.makeConstraints {(make) in
          make.top..equalToSuperView().inset(80)
          make.trailing.equalToSuperView().inset(30)
      }

      self.naverBtn.addTarget(self, action: #selector(self.naverLogin(_:)), for: .touchUpInside)
      self.naverLogoutBtn.addTarget(self, action: #selector(self.naverLogoutBtn(_:)), for: .touchUpInside)
  }
}

extension ViewController: NaverThirdPartyLoginConnectionDelegate {
    @objc func naverLogin(_ sender: Any) {
        naverConnection?.delegate = self
        naverConnection?.requestThirdPartyLogin()
    }

    @objc func naverLogoutBtn(_ sender: Any) {
        naverConnection?.requestDeleteToken()
    }

    func oauth20ConnectionDidFinishRequestACTokenWithAuthCode() {
        print("Success Login")
        self.getInfo()
    }

    func oauth20ConnectionDidFinishRequestACTokenWithRefreshToken() {
        self.getInfo()
    }

    func oauth20ConnectionDidFinishDeleteToken() {
        print("logout")
    }

    func oauth20Connection(_ oauthConnection: NaverThirdPartyLoginConnection!, didFailWithError error: Error!) {
        print("error = \(error.localizedDescription)")
    }

    func getInfo() {
        // 현재 토큰이 유효한지 확인 > default로 1시간
        guard let isValidAccessToken = naverConnection?.isValidAccessTokenExpireTimeNow() else { return }

        if !isValidAccessToken {
            return
        }

        guard let tokenType = naverConnection?.tokenType else { return }
        guard let accessToken = naverConnection?.accessToken else { return }

        let urlStr = "https://openapi.naver.com/v1/nid/me"
        let url = URL(string: urlStr)!

        let authorization = "\(tokenType) \(accessToken)"
        let req = AF.request(url, method: .get, parameters: nil,
          encoding: JSONEncoding.default, headers: ["Authorization": authorization])

        req.responseJSON {(response) in
            print(response)

            guard let result = response.value as? [String: AnyObject] else { return }
            guard let object = result["response"] as? [String: AnyObject] else { return }
            let name = object["name"] as? String
            let id = object["id"] as? String
            let image = object["profile_image"] as? String

            print("name: ", name ?? "no name")
            print("id: ", id ?? "no id")
            print("image: \(image)")
        }
    }  
}
```


- oauth20ConnectionDidFinishRequestACTokenWithAuthCode(): 로그인에 성공했을 경우 호출되는 함수
- oauth20ConnectionDidFinishRequestACTokenWithRefreshToken(): 접근 토큰을 갱신할 때 호출되는 함수
- oauth20ConnectionDidFinishDeleteToken(): 토큰 삭제를 하면 호출되는 함수(로그아웃에 사용)
- oauth20Connection(_ oauthConnection: NaverThirdPartyLoginConnection!, didFailWithError error: Error!): 네아로에 모든 에러에 호출되는 함수


## 4.Error Solution


[카카오, 네이버, 페이스북 소셜 로그인 연동시 발생했던 에러 정리](https://www.zehye.kr/ios/2021/09/24/iOS_social_login/)를 참고해주세요!

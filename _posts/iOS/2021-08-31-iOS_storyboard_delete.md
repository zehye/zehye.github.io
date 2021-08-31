---
layout: post
title: Xcode Storyboard 삭제 및 Scene delegate 삭제하는 방법
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

회사에서는 모든 UI를 코드로 짜고있다. 스토리보드를 무척 사랑했던 나로서는 정말 슬픈일이었지만, 어쩌겠나요!<br>
앞으로는 코드로 UI를 짜는것이 습관화 되어야 하기에 개인 실습 프로젝트를 하나 파게 되었고 <br>
그러면서 XCode에서 스토리보드와 Scene delegate를 샂게하는 방법을 같이 정리해보려고 합니다!



## Storyboard 삭제하기

스토리보드를 제거하기 위해서는 프로젝트가 생성되는 Main.Storyboard와의 연동된 부분을 끊으면 된다.

### Info.plist에서 Main Storyboard file base name > Main을 지운다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/20.png" alt="" width="50%">
</figure>
</center>


혹은 프로젝트 설정에서 **[Main Interface]에서 Main을 지우면** Info.plist에도 반영된다. 이 방법을 사용해도 같이 반영된다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/21.png" alt="" width="50%">
</figure>
</center>


### Main.Storyboard 파일을 삭제한다.

해당 파일은 더이상 사용하지 않기 때문에 삭제해도 괜찮다.



## SceneDelegate 삭제하기

기존에 SceneDelegate에서 UIWindow를 설정하는 부분을 예전처럼 AppDelegate로 옮기고, Scene관련 파일과 설정을 제거한다.


### AppDelegate 에서 Scene 관련 함수 정의부를 제거한다.


```swift
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



### AppDelegate에 UIWindow 설정 로직을 추가한다.

```swift
import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

        window = UIWindow()
        window?.rootViewController = ViewController()
        window?.makeKeyAndVisible()

        return true
    }    
}
```


### SceneDelegate 파일을 삭제한다.

해당 파일은 더이상 사용하지 않기에 삭제하도록 한다.


### Info.plist에서 Application Scene Manifest 항목을 통쨰로 삭제한다.


<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/22.png" alt="" width="50%">
</figure>
</center>


### 확인해보기

ViewController의 기본뷰에 배경색을 입히고 앱을 실행시켜 적용한 배경색이 잘 뜨는지 확인해보자.


```swift
import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.

        view.backgroundColor = .blue
    }
}
```

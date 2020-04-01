---
layout: post
title: 코코아 프레임워크, 코코아 터치, 코코아 팟이란?(설치 후 사용까지!)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.   
[참고한 블로그](https://developer-fury.tistory.com/6)

<hr>

## Framework

소프트웨어의 구체적인 부분에 해당하는 설계와 구현을 재사용이 가능하게끔 일련의 협업화된 형태로 클래스를 제공하는 것

클래스와 그에 속하는 메서드들을 미리 만들어놓고 필요할때마다 사용하는 라이브러리와 비슷하다고 생각이 들었다. 라이브러리는 필요한 사오항에서 원하는 메서드를 뽑아쓸 수 있도록 즉 완성된 메서드들이 구성되어있는 것을 의미힌다. 이에 반해 프레임워크는 개발자가 자신의 애플리케이션에서 틀이 짜여진 미완성의 메서드를 채워넣을 수 있는 형태로 정의되어있다(자바의 추상메서드 implement와 비슷) 또한 프레임워크는 개발을 하기위한 확장성을 가지는 코드를 포함하고 있다. 이것만 봐도 프레임워크의 역할은 프레임워크에서 내 어플리케이션에 메서드를 가져와 원하는 앱 개발 작업을 할 수 있도록 도와주는 것이라고 알 수 있겠다.

즉, **프레임워크는 앱개발을 편리하고 빠르게 할 수 있도록 뼈대를 미리 세워두는것**, 살을 붙일 때 필요한 라이브러리 라고 생각할 수 있겠다.

스위프트에서의 약간 프로토콜과 비슷한 개념이라고도 생각했다.


## Cocoa Framework

위에서 언급했듯 프레임워크는 앱의 뼈대를 만들어두는 것이고 이런 **프레임워크를 여러개 모아 더욱 큰 프레임워크도 구성한 것이 애플의 코코아프레임워크** 이다.

이중 터치와 관련된 디바이스의 애플리케이션을 개발할 때 사용하는 도구가 코코아터치 프레임워크인 것이다. 보통 iOS를 개발할 때 이 코코아 터치 프레임워크를 사용하게 된다. 그리고 이 프레임워크에 우리가 iOS 개발할 때 가장 많이 마주치게되는 UIKit, Foundation 프레임워크가 포함되어 있다. 이를 사용할때는 간단하게 import해주면 된다. Foundation은 말그대로 기반을 위한 도구들이 포함되어있다.(데이터타입, 콜렉션, OS 기능 등) UIKit는 iOS 를 위한 기반을 제공하며 인터페이스 그래픽을 구성하는 도구를 포함하고 있다.


## CocoaPods

> [CocoaPods 공식사이트](https://cocoapods.org/)     
CocoaPods is a dependency manager for Swift and Objective-C Cocoa projects. It has over 63 thousand libraries and is used in over 3 million apps. CocoaPods can help you scale your projects elegantly.

코코아팟은 Swift와 Objective-C 코코아 프로젝트를 위한 의존적인 매니저다. 굉장히 많은 라이브러리를 가지고 있으며 굉장히 많은 애플리케이션에 사용되고 있다. 당신의 프로젝트가 우아하게 돌아가도록 도와줄 수 있다.

만약 코코아팟 없이 프로젝트를 만들면, 오픈소스 프레임워크중에 원하는 기능이 있을 때 해당 프레임워크를 수동으로 설치해야하며, 프로젝트 안에 로드시키는 과정이 반드시 필요하다. 빌드 설정또한 우리가 직접 건들여줘야하며, 버전이 업데이트되면 위 작업들을 다시 새로해야하는 불편함또한 생긴다. 이런 불편함등을 해소하기 위해 만든것이 코코아팟이다. 코코아팟에 사용하고자 하는 라이브러리/프레임워크의 목록을 텍스트로 작성해두면 알아서 설치와 업데이트를 진행하고 Xcode 프로젝트와의 연결 및 설정도 도와준다.



### Installation

코코아팟은 일단 Ruby로 제작되었다. 따라서 mac에서 설치하기 위해 아래의 명령어를 작성해줘야한다.

```vim
sudo gem install cocoapods
```

sudo 명령어는 일단 함부로 쓰면 안되기 떄문에 멈칫- 했지만 해야한다고 하니 일단 해보자.<br>
함부로 쓰면 안되는 가장 기본적인 이유는 sudo를 사용하게 되면 시스템 전역에 설치가 되기 때문이다. 이렇게 함부로 모든 파일들을 sudo를 통해 설치하게 되면 기존 설치된 애들과 의존성문제로 꼬이게 되고 그러다보면 어디서부터 뭐가 잘못된지 모르게 안되는 것들이 하나, 둘 생기게 되는데 이를 해결하는 방법또한 찾을수가 없게된다.(이럴때는 결국 초기화가 답..그래서 정말 어지간한거는 무조건 brew로 설치하는게 가장가장가장!!!!!!!!!좋다!) 이렇게 설치하면 일단 코코아팟 설치가 된것이다.

쓰다보며 확인해봤는데(나는 아직 설치하기 전이었음) brew에도 cocoapods이 있더군요!

```vim
brew search cocoapods
brew cask install cocoaPods
```


### Xcode 프로젝트에서 CocoaPods 사용해보기

설치가 끝났으니 이제 프로젝트에서 코코아팟을 사용해보도록 합시다.

터미널을 통해 해당 프로젝트가 있는 폴더로 이동한 뒤 해당 디렉토리에서 아래 명령어를 실행한다.

```vim
pod init
```

해당 명령어를 실행하고 나면 **Podfile** 파일이 생성된 것을 볼 수 있을 것이다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/1.png" alt="" width="80%">
</figure>
</center>

이제 Podfile에 사용할 라이브러리 의존성을 추가할 것이다.

```vim
vi Podfile
```

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/2.png" alt="" width="80%">
</figure>
</center>

설치가 완료되고나면 아래와 같은 노란글씨가 보일 수 있다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/3.png" alt="" width="80%">
</figure>
</center>

이건, Podfile을 보면 "# platform :ios, '9.0'이라는 문구가 주석 처리되어있는데, Platform Version을 작성해주지 않고 주석처리를 하였기 때문에 CocoaPods에서 자동적으로 12.2 버전으로 assigning 했다는 경고로 직접 버전을 작성해 주셔야 한다면 주석을 풀어주면 된다.


이제 사용하고자 하는 Naver Map에 대한 라이브러리 설치는 완료됐다! (간-단)

자! 지금부터 주의해야하는 점은 아래와 같다.

우리는 지금까지 프로젝트를 열때 **.xcodeproj** 라는 확장자 파일을 실행했는데 코코아팟을 통해 라이브러리를 설치한 이후부터는 무조건! **xcworkspace** 라는 확장자 파일을 실행해야만 한다.

```vim
open cocoaPodsPractice.xcworkspace
```

이러고 열려진 xcode를 확인해보면 아래와 같이 기존과 동일한 프로젝트 파일 외에 Pods라는 것이 들어있는 것을 볼 수 있을 것이다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/4.png" alt="" width="80%">
</figure>
</center>

이제 우리가 적용한 Naver Map을 Import하여 사용해보도록 하자!

```swift
import UIKit
import NMapsMap

class ViewController: UIViewController {

    var authState: NMFAuthState!

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.

        let nmapView = NMFMapView(frame: view.frame)
        view.addSubview(nmapView)
    }
}
```

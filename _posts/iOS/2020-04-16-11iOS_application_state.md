---
layout: post
title: iOS Application state(iOS 앱의 생명주기)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.     

<hr>


일반적으로 생명주기를 생각해보면 우리가 홈버튼을 누르거나, 전화가 왔을때와 같이 앱이 화면상에 보이지 않는 **Background** 상태, 화면에 앱이 올라와있는 **Foreground** 상태를 떠올릴 수 있다. 제대로 된 앱을 만들기 위해서는 이 생명주기를 정확히 이해하는 것이 중요하며, 이러한 생명주기를 이해하기 이전에 iOS앱이 시작할 때 일어나는 일들을 짧게나마 이해하는 것 또한 중요하다. >> [참고할 글: View Controller의 생명주기](https://www.zehye.kr/ios/2020/02/12/11iOS_viewcontroller_lifecycle/)

더 근본적으로 이야기해서 생명주기에 대해 간략하게 정리해보자면,

생명주기라는 것은 앱의 최초 실행부터 앱이 완전히 종료되기까지 앱이 가지는 상태와 그 상태들 사이의 전이를 뜻한다. 앱의 상태는 앱이 현재 어떠한 것을 할 수 있는 가를 결정한다. 예로 들어, Foreground 상태의 애플리케이션은 유저와 직접 상호작용하기 때문에 시스템 자원 사용 등에서 우선순위를 갖는다. 반면 Background 상태의 앱은 스크린에 드러나지 않기 때문에 작업을 하지 않거나, 하더라도 최소한의 작업만 수행해야 한다. 앱의 상태가 변하게 되면, 이 상태에 따라 앱의 행동을 조절해줘야 하는데, 이 조절작업을 하기 위해 UIKit는 특정 위임객체(delegate object)에 메시지를 보내주게 된다.

> iOS 13이후에는 SceneDelegate를 사용할 수 있다. 이는 햐나의 앱이 여러개의 UI를 띄울 수 있게 되면서, 이 UI가 별도의 생명주기를 가질 필요가 생겼기 때문으로 기존 AppDelegate가 수행했던 생명주기 관리의 많은 부분이 SceneDelegate로 옮겨가게 되었다. 여기서 AppDelegate는 앱의 초기 구동과 앱 전체에 관련된 이벤트의 처리정도 만을 담당하도록 기능이 축소되었다.       
iOS 12 이하의 경우에는 여전히 생명주기 관리에 AppDelegate를 사용하며, iOS 13 이상이더라도 Scene을 지원하지 않게 만들었다면 여전히 AppDelegate를 사용한다.

Scene을 지원하지 않는 경우, 모든 생명주기 관련 이벤트들은 appDelegate에 전달된다. appDelegate객체는 앱의 모든 window를 관리하기 때문에 앱의 상태 변화는 앱의 모든 UI에 영향을 미친다.


## Application state

iOS에서 앱은 5가지의 상태로 구분이 가능하며 항상 하나의 상태를 가지고 있다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/26.png" alt="" width="50%">
</figure>
</center>

- Not running(Unattached): 앱이 실행되지 않았거나 시스템에서 종료됨
- In-Active: 앱이 Foreground에서 실행중이지만, 이벤트를 받을 수 없음 > 이 상태에 잠시 머물다가 다른 상태로 전이됨
- Active: 앱이 화면에 떠 있을 때의 상태 > 이벤트를 받을 수 있음
- Background: 앱이 백그라운드에서 코드를 실행중. 대부분의 앱은 일시중지 상태로 잠시 이 상태가 된다. 그러나 추가 시간을 요청하는 앱은 일정기간 동안 이 상태로 남아있을 수 있다. 또한 백그라운드로 직접 실행되는 앱은 비활성 상태 대신 이 상태로 전환된다
- Suspended: 앱이 백그라운드에 있지만 코드를 실행하지 않음. 시스템은 앱을 자동으로 이 상태로 이동시키고 그렇게 하기 전에 앱에 알리지 않는다. 일시 중지 된 앱은 메모리에 남아는 있지만 코드는 실행되지 않는다. 메모리 부족상태가 발생하면 시스템은 예공ㅂㅅ이 일시 중단 된 앱을 제거하여 메모리를 확보한다.

사용자나 시스템이 새로운 scene을 만들어달라고 요청하면, UIKit는 이를 만들어 unattached 상태로 만든다. 사용자가 요청한 scene은 곧장 Foreground inactive 상태를 거쳐 active 상태가 되고, 시스템이 요청한 scene은 보통 Background 상태가 되어 이벤트를 처리한 뒤에 Foreground로 올라온다. 사용자가 앱의 UI를 닫게 되면, 해당하는 scene은 UIKit에 의해 Background 상태로 갔다가 Suspended 상태가 된다. UIKit는 언제든지 이 Background나 Suspended 상태인 scene을 회수해 unattached 상태로 되돌려 놓을 수 있다.

### View Controller Life Cycle

앱의 라이프사이클은 앱을 터치해서 실행시키고 완전히 종료되기 까지 아래 단계로 요약 해볼 수 있다.

1. main 함수 실행
2. main 함수는 UIApplicationMain 함수를 호출
3. UIApplicationMain함수는 앱의 본체에 해당하는 객체인 UIApplication 객체를 생성
4. nib 파일을 사용하는 경우나, Info.plist 파일을 읽어들여 파일에 기록된 정보를 참고해 그 외에 필요한 데이터를 로드
5. App Delegate 객체를 만들고 앱 객체와 연결해 Main를 만드는 등 실행에 필요한 준비
6. 실행 완료를 앞두고 앱 객체가 App Delegate에게 application:didFinishLaunchingWithOptions: 메시지를 보냄


### 상태전이

사용자의 동작에 따라 앱의 상태가 변경된다.

1. 사용자가 앱을 실행한다: Not running >> In-Active >> Active
2. 앱 실행 도중 홈버튼을 누른다: Active >> In-Active >> Background
3. 앱을 다시 켠다: Background >> Active
4. 앱이 백그라운에 있다가 Suspended 상태로 전이: Active >> In-Active >> Background >> Suspended



## 앱이 In-Active 상태가 되는 시나리오를 설명해보자

우선 일반적으로 이런경우는 흔하지 않다.

가장 흔한 예시는 우리가 네이버에서 기사를 읽고있을때 기사를 카카오톡으로 공유하려고 해보자.<br>
이때 네이버 앱 위로 카카오톡 앱이 실행되는데, 이때 네이버 앱은 In-Active 상태가 되어진다고 생각하면 된다.

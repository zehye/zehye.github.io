---
layout: post
title: Navigation Interface
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Navigation Interface

iOS에서 내비게이션 인터페이스는 주로 **계층적 구조의 화면전환을 위해 사용되는 드릴 다운 인터페이스(drill-down interface)** 이다. 드릴 다운 인터페이스란 아래 그림과 같이 각 선택할 수 있는 항목에 대한 세부항목이 존재하는 인터페이스를 의미한다. 내비게이션 인터페이스는 **내비게이션 컨트롤러를 통해 구현** 한다.

<center>
<figure>
<img src="/assets/post-img/iOS/1.png" alt="" width="80%">
</figure>
</center>

> 정보의 흐름, 깊이를 가진다.

### 내비게이션 컨트롤러

내비게이션 컨트롤러는 **컨테이너 뷰 컨트롤러로써(container view controller)** 내비게이션 스택(navigation stack)을 사용하여 다른 뷰 컨트롤러를 관리한다. 여기서 내비게이션 스택에 담겨서 콘텐츠를 보여주게 되는 뷰 컨트롤러들을 **컨텐트 뷰 컨트롤러(content view controller)** 라고 한다.

내비게이션 컨트롤러는 두 개의 뷰를 화면에 표시한다.

1. 하나는 내비게이션 스택뷰에 포함된 최상위 컨텐트 뷰 컨트롤러의 콘텐츠를 나타내는 뷰
2. 나머지는 내비게이션 컨트롤러가 직접 관리하는 뷰(내비게이션바 또는 툴바)

추가로 내비게이션 인터페이스의 변화에 따른 특정 액션을 동작하도록 하기 위해 **내비게이션 델리게이트 객체** 를 사용할 수 있다.

<center>
<figure>
<img src="/assets/post-img/iOS/2.png" alt="" width="80%">
</figure>
</center>


### 내비게이션 스택이란?

내비게이션 컨트롤러에 의해 관리되는 내비게이션 스택(Navigation stack)은 **뷰 컨트롤러를 담을 수 있는 배열** 과도 같다. 내비게이션 스택에 가장 하위에 있는(가장 먼저 스택에 추가된) 뷰 컨트롤러는 내비게이션 컨트롤러의 **루트 뷰 컨트롤러(root view controller)** 가 된다. 루트 뷰 컨트롤러는 내비게이션 스택에서 팝(pop)되지 않는다. 내비게이션 스택의 가장 상위에 있는(가장 마지막에 푸시(push) 된) 뷰 컨트롤러는 최상위 뷰 컨트롤러로 화면에 보이게 된다.

<center>
<figure>
<img src="/assets/post-img/iOS/3.png" alt="" width="80%">
</figure>
</center>

이름에서도 알 수 있듯이 내비게이션 스택은 푸시(push)/팝(pop)을 통하여 아이템(뷰 컨트롤러)을 관리한다. 새로운 뷰 컨트롤러를 내비게이션 스택에 푸시(push)하거나 내비게이션 스택에 있는 뷰 컨트롤러를 삭제하기 위해 팝(pop)을 사용하며, 내비게이션 스택에 푸시(push) 된 각 뷰 컨트롤러들은 애플리케이션에 자신이 가지고 있는 뷰 계층 구조를 통해 콘텐츠를 표시하게 된다.


### 내비게이션 스택에서의 화면이동

UINavigationController 클래스의 메서드 또는 세그(segue)를 사용하여 내비게이션 스택의 뷰 컨트롤러를 추가/삭제할 수 있다. 또한 애플리케이션 실행 중 사용자가 내비게이션 인터페이스의 뒤로가기(back) 버튼을 사용하거나 화면의 왼쪽 가장자리를 스와이프(swipe)하여 스택에 있는 최상위 뷰 컨트롤러를 삭제하고 그 아래에 가려져 있던 뷰 컨트롤러의 콘텐츠를 보여줄 수도 있다.
(세그(segue)는 스토리보드에서 한 화면에서 다른 화면으로의 전환을 의미한다. 세그도 내부적으로 UINavigationController 클래스의 메서드를 사용)

이해를 돕기 위한 간단한 예를 통해 내비게이션 스택의 상태에 따라 어떻게 화면이 이동하는지 보자.

<hr>

#### 내비게이션 스택의 푸시(push)

내비게이션 스택에 새로운 뷰 컨트롤러가 푸시 될 때 **UIViewController 인스턴스가 생성되고 내비게이션 스택에 추가** 된다.

1.가장 먼저 내비게이션 스택에 루트 뷰 컨트롤러만 들어가 있는 초기상태이다 (내비게이션 컨트롤러를 생성할 때 반드시 루트 뷰 컨트롤러가 설정되어 있어야 한다)

<center>
<figure>
<img src="/assets/post-img/iOS/4.png" alt="" width="80%">
</figure>
</center>

2.'뷰 컨트롤러1로 이동'이라는 버튼을 통해서 내비게이션 스택에 뷰 컨트롤러1을 푸시(push)!<br>
뷰 컨트롤러1의 인스턴스가 생성되고 내비게이션 스택에 추가됨과 동시에 뷰 컨트롤러1이 최상위 뷰 컨트롤러로써 화면에 보이게 된다.

<center>
<figure>
<img src="/assets/post-img/iOS/5.png" alt="" width="80%">
</figure>
</center>

3.'뷰 컨트롤러2로 이동'이라는 버튼을 통해서 내비게이션 스택에 뷰 컨트롤러2도 푸시한다!<br>
뷰 컨트롤러2의 인스턴스가 생성되고 내비게이션 스택에 추가되며 뷰 컨트롤러2가 최상위 뷰 컨트롤러로써 화면에 보이게 된다. 여기서 주목할 점은 새로운 뷰 컨트롤러가 추가될 때도 **아래에 있는 뷰 컨트롤러들이(인스턴스) 삭제되지 않고 유지** 되고 있다는 점이다.

<center>
<figure>
<img src="/assets/post-img/iOS/6.png" alt="" width="80%">
</figure>
</center>


#### 내비게이션 스택의 팝(pop)

내비게이션 스택에 존재하는 뷰 컨트롤러가 팝 될 때 생성되었던 UIViewController의 인스턴스는 다른 곳에서 참조되고 있지 않다면 메모리에서 해제되고, 내비게이션 스택에서 삭제된다.

1.푸시 예제의 마지막 상태에서 상단 내비게이션바에 있는 뷰 컨트롤러1을(back button) 눌러서 뷰 컨트롤러2를 팝한다.<br>
뷰 컨트롤러2가 내비게이션 스택에서 삭제되며 뷰 컨트롤러1이 다시 최상위 뷰 컨트롤러로써 화면에 보여지게 된다.

<center>
<figure>
<img src="/assets/post-img/iOS/7.png" alt="" width="80%">
</figure>
</center>

2.내비게이션바에서 루트 뷰 컨트롤러를(back button) 눌러서 뷰 컨트롤러1을 팝한다.<br>
뷰 컨트롤러1이 메모리에서 해제되고 내비게이션 스택에서 삭제되며 루트 뷰 컨트롤러가 최상위 뷰 컨트롤러가 되고 화면에 보여지게 된다. 루트 뷰 컨트롤러는 내비게이션 스택에서 팝 되지 않으며 상단에 내비게이션바를 통해서도 루트 뷰 컨트롤러를 팝 하는 버튼이 따로 생성되어 있지 않은 것을 확인할 수 있다.

<center>
<figure>
<img src="/assets/post-img/iOS/8.png" alt="" width="80%">
</figure>
</center>


### UINavigationController 클래스

위에서 설명한 내비게이션 컨트롤러와 내비게이션 스택의 동작들이 UINavigationController 클래스에서 어떻게 구현되어있는지 살펴본다.


#### 내비게이션 컨트롤러의 생성

```swift
// 내비게이션 컨트롤러의 인스턴스를 생성하는 메서드
// 매개변수로 내비게이션 스택의 가장 아래에 있는 루트 뷰 컨트롤러가 될 뷰 컨트롤러를 넘겨준다
init(rootViewController: UIViewController)
```

#### 내비게이션 스택의 뷰 컨트롤러에 대한 접근

```swift
// 내비게이션 스택에 있는 최상위 뷰 컨트롤러에 접근하기 위한 프로퍼티
var topViewController: UIViewController?

// 현재 내비게이션 인터페이스에서 보이는 뷰와 관련된 뷰 컨트롤러에 접근하기 위한 프로퍼티
var visibleViewController: UIViewController?

// 내비게이션 스택에 특정 뷰 컨트롤러에 접근하기 위한 프로퍼티(루트 뷰 컨트롤러의 인덱스는 0)
var viewController: [UIViewController]
```


#### 내비게이션 스택의 푸시와 팝에 관한 메서드

```swift
// 내비게이션 스택에 뷰 컨트롤러를 푸시
// 푸시 된 뷰 컨트롤러는 최상위 뷰 컨트롤러로 화면에 표시된다
func pushViewController(UIViewController, animated: Bool)

// 내비게이션 스택에 있는 최상위 뷰 컨트롤러를 팝
// 최상위 뷰 컨트롤러 아래에 있던 뷰 컨트롤러의 콘텐츠가 화면에 표시된다
func popViewController(animated: Bool) -> UIViewController?

// 내비게이션 스택에서 루트 뷰 컨트롤러를 제외한 모든 뷰 컨트롤러를 팝
// 루트 뷰 컨트롤러가 최상위 뷰 컨트롤러가 된다
// 삭제된 모든 뷰 컨트롤러의 배열이 반환
func popToRootViewController(animated: Bool) -> [UIViewController]?

// 특정 뷰 컨트롤러가 내비게이션 스택에 최상위 뷰 컨트롤러가 되기 전까지 상위에 있는 뷰 컨트롤러들을 팝
func popToViewController(_ viewController: UIViewController,
		animated: Bool) -> [UIViewController]?
```


### 내비게이션 인터페이스를 구성하는 두 가지 방법

#### 스토리보드를 사용하여 내비게이션 인터페이스 구성하기

1. 스토리보드에서 내비게이션 컨트롤러에 포함할 뷰 컨트롤러를 선택
2. 메뉴에서 [Editor] - [Embed In] - [Navigation Controller]를 차례로 선택
3. 선택한 뷰 컨트롤러가 내비게이션 컨트롤러의 루트 뷰 컨트롤러가 되면서 내비게이션 컨트롤러가 생성됨
4. 위의 방법 외에도 객체 라이브러리에서 내비게이션 컨트롤러를 드로그 앤 드롭해서 캔버스에 올릴 경우 테이블 뷰를 포함한 루트 뷰 컨트롤러가 생성되면서 내비게이션 컨트롤러가 만들어짐



#### 코드작성을 통해 내비게이션 인터페이스 구성하기

코드로 내비게이션 컨트롤러를 생성할 경우, 내비게이션 컨트롤러가 생성되기 원하는 적절한 지점에 내비게이션 컨트롤러를 생성할 수 있다. 예를 들어 내비게이션 컨트롤러가 애플리케이션 윈도우(window)의 루트 뷰로서 역할을 한다면, 내비게이션 컨트롤러를 `applicationDidFinishLaunching:` 메서드에 구현할 수 있다.

1. 루트 뷰 컨트롤러가 될 뷰 컨트롤러를 생성 > 이 객체는 처음에 내비게이션 스택의 최상위 뷰 컨트롤러가 화면에 보이게 되고 내비게이션 바에 뒤로가기 버튼이 생성되지 않는다
2. `init(rootViewController: UIViewController)` 메서드를 통해 내비게이션 컨트롤러를 초기화하고 생성
3. 내비게이션 컨트롤러를 윈도우의 루트 뷰 컨트롤러로 설정

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.

        // 루트 뷰 컨트롤러가 될 뷰 컨트롤러를 생성
        let rootViewController = UIViewController()
        // 위에서 생성한 뷰 컨트롤러로 내비게이션 컨트롤러를 생성
        let navigationController = UINavigationController(rootViewController: rootViewController)

        self.window = UIWindow(frame: UIScreen.main.bounds)
        // 윈도우의 루트 뷰 컨트롤러로 내비게이션 컨트롤러를 설정
        self.window?.rootViewController = navigationController
        self.window?.makeKeyAndVisible()

        return true
    }
```

### 내비게이션바의 구성

내비게이션바는 **내비게이션 컨트롤러에 의해 생성** 된다.

내비게이션바는 내비게이션 컨트롤러의 관리를 받는 **모든 뷰 컨트롤러의 상단에 표시** 되며 최상위 뷰 컨트롤러가 변경될 때마다 내비게이션 컨트롤러는 내비게이션바를 업데이트한다.


<center>
<figure>
<img src="/assets/post-img/iOS/9.png" alt="" width="80%">
</figure>
</center>

내비게이션바는 다음과 같은 구조로 이루어져 있다.

<center>
<figure>
<img src="/assets/post-img/iOS/10.png" alt="" width="80%">
</figure>
</center>

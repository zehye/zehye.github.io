---
layout: post
title: iOS15 업데이트 후 NavigationBar 대응
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## iOS15

아이폰13이 나오고 iOS15가 발표되고 난 후 큰 틀이 바뀌진 않았지만 그중 큰 변화로는 네비게이션 바의 변화였습니다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/24.png" alt="" width="80%">
</figure>
</center>

iOS15로 업데이트가 된 이후부터는 네비게이션 바가 확장이 되면서 `scrollEdgeAppearance`는 기본적으로 투명한 배경으로 생성이 되면서 뒤에 컨텐츠가 없는 경우 기본적으로 투명한 배경색으로 보이게 변하였습니다. 따라서 실제로 앱이 Xcode13에서 빌드를 하게 되면, 배경을 가진 이전 'bar'는 사라지고 콘텐츠를 스크롤 하는 경우에 다시 'bar'가 보이게 되는 것을 확인할 수 있습니다. 이러한 변화가 기본적으로 활성화되어있기 때문에, 앱에서 시각적인 문제가 발생할 수 있게 되었습니다. 이에 대한 해결 방법은 iOS13부터 사용이 가능한 `UINavigationBarAppearance`를 사용해야합니다.

```swift
if #available(iOS 15.0, *) {
    let navigationBarAppearance = UINavigationBarAppearance()
    navigationBarAppearance.configureWithDefaultBackground()

    UINavigationBar.appearance().standardAppearance = navigationBarAppearance
    UINavigationBar.appearance().compactAppearance = navigationBarAppearance
    UINavigationBar.appearance().scrollEdgeAppearance = navigationBarAppearance
}
```

- configureWithDefaultBackground는 네비게이션 바를 반투명하게 표시하도록 설정합니다.

여기서 핵심은 `scrollEdgeAppearance`를 `standardAppearance`와 동일하게 설정해야한다는 것입니다. UINavigationBar.appearance()로 앱의 모든 네비게이션 바에 대한 자동 투명도가 해제될 수 있게 됩니다. 위 코드가 이상적으로 실행되기 위해서는 `AppDelegate`에서 실행되는 것이 좋습니다.

전체적으로 네비게이션 바의 자동 투명도를 해제시킨다면, 한가지 문제가 발생할 수 있습니다.

네비게이션으로 이루어진 각각의 화면들에서 네비게이션 바를 다르게 처리해야할 때가 있을 수 있기 때문이죠.<br>
그때의 해결방법은 아래와 같습니다.

```swift
if #available(iOS 15.0, *) {
    if isTransparent {
        let navigationBarAppearance = UINavigationBarAppearance()
        navigationBarAppearance.configureWithTransparentBackground()

        navigationController.navigationBar.tintColor = .white

        navigationItem.scrollEdgeAppearance = navigationBarAppearance
        navigationItem.standardAppearance = navigationBarAppearance
        navigationItem.compactAppearance = navigationBarAppearance

    } else {
        let navigationBarAppearance = UINavigationBarAppearance()
        navigationBarAppearance.configureWithDefaultBackground()

        navigationController.navigationBar.tintColor = .blue

        navigationItem.scrollEdgeAppearance = navigationBarAppearance
        navigationItem.standardAppearance = navigationBarAppearance
        navigationItem.compactAppearance = navigationBarAppearance
    }

    navigationController.setNeedsStatusBarAppearanceUpdate()
}
```

코드에 적용된 tintColor는 단지 예시용이며, 자신의 앱에 맞게 사용하시면 됩니다.

스토리보드에서도 마찬가지로 standard, scroll edge의 backgroundColor를 동일하게 지정해주면 됩니다.

1. NavigationBar > Appearance에서 standard, scroll edge 선택
2. Standard Appearance, Scroll Edge Appearance의 Background 색상 설정


이때 `standard`의 background color는 스크롤을 하고 있는 도중의 색상을 의미하고 `scroll edge`의 background color는 스크롤을 하기 전 색상을 의미합니다.


<hr>

네비게이션을 사용할때 쓰이는 용어를 약간! 정리해보았습니다.

### tintColor

네비게이션 아이템들과 바 버튼 아이템에 적용되는 색을 의미합니다.

```swift
self.navigationController?.navigationBar.tintColor =  // 원하는 색상
```



### barTintColor

네비게이션바의 백그라운드에 적용되는 색을 의미합니다.<br>
`isTranslucent`를 false로 설정하지 않는다면 기본값은 반투명입니다.

```swift
self.navigationController?.navigationBar.barTintColor =  // 원하는 색상
self.navigationController?.navigationBar.isTranslucent = false
```




### background

네비게이션 바에 적용되는 색을 의미합니다.

```swift
self.navigationController?.navigationBar.backgroundColor =  // 원하는 색상
```

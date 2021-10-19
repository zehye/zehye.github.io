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

![NavigationBar]({{ site.url }}{{ site.baseurl }}/images/2021/iOS/navi1.png)

iOS15로 업데이트가 된 이후부터는 네비게이션 바가 확장이 되면서 `scrollEdgeAppearance`는 기본적으로 투명한 배경으로 생성이 되면서 UINavigationBarAppearance가 아닌 UINavigationBar를 채택해 옵션을 생성해주어야 한다고 합니다.

```swift
let appearance = UINavigationBarAppearance()
appearance.configureWithOpaqueBackground()  // 불투명하게
appearance.backgroundColor = // 원하는 색상으로

navigationBar.standardAppearance = appearance
navigationBar.scrollEdgeAppearance = navigationBar.standardAppearance
```

스토리보드에서도 마찬가지로 standard, scroll edge의 backgroundColor를 동일하게 지정해주면 됩니다.

1. NavigationBar > Appearance에서 standard, scroll edge 선택
2. Standard Appearance, Scroll Edge Appearance의 Background 색상 설정


이때 `standard`의 background color는 스크롤을 하고 있는 도중의 색상을 의미하고 `scroll edge`의 background color는 스크롤을 하기 전 색상을 의미합니다.



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

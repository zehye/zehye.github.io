---
layout: post
title: Navivation Item, Bar Item
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 내비게이션 아이템(NavigationItem)

내비게이션 아이템은 내비게이션바의 콘텐츠를 표시하는 객체로, 뷰 컨트롤러가 전환될 때마다 내비게이션바는 하나의 공동 객체지만 내비게이션 아이템은 각각의 뷰 컨트롤러가 가지고 있는 프로퍼티이다.
즉, 내비게이션바가 내비게이션 컨트롤러와 연관이 있다면 내비게이션 아이템은 해당 뷰 컨트롤러와 연관이 있으며 보통 내비게이션바에서 보여지는 중앙 타이틀, 좌측 바 버튼, 우측 바 버튼 등이 내비게이션 아이템의 프로퍼티이다.

<center>
<figure>
<img src="/assets/post-img/iOS/105.png" alt="" width="80%">
</figure>
</center>

#### 주요 프로퍼티

- title : 내비게이션바에 표시되는 내비게이션 아이템의 제목. 기본값은 nil
```swift
var title: String? { get set }
```
- backBarButtonItem : 내비게이션바에서 뒤로 버튼이 필요할 때 사용할 바 버튼 항목
```swift
 var backBarButtonItem: UIBarButtonItem? { get set }​
```
- hidesBackButton : 뒤로 버튼이 숨겨져 있는지를 결정하는 부울 값
```swift
var hidesBackButton: Bool { get set }
```


#### 주요 메서드

```swift
// setHidesBackButton(_:animated:) : 뒤로 버튼이 숨겨져 있는지를 설정하고, 전환 애니메이션 효과 적용
func setHidesBackButton(_ hidesBackButton: Bool, animated: Bool)​
```


### 내비게이션 아이템 커스터마이징

내비게이션 아이템은 크게 좌, 우 바 버튼 아이템과 중앙 타이틀 영역이 있다.<br>
좌측 바 버튼, 우측 바 버튼 아이템의 경우 타입은 UIBarButtonItem 이며 상황에 따라서 커스텀 뷰를 넣어서 구현할 수도 있다.



#### 프로퍼티

```swift
// titleView : 중앙 타이틀 영역의 뷰
 var titleView: UIView? { get set }

// Tip : 중앙 타이틀 영역에 뷰를 상속받은 (UILabel, UIView, UIImageView 등등) 클래스들을 표현할 수 있습니다.
leftBarButtonItems : 좌측 아이템 영역의 UIBarButtonItem의 바 버튼 아이템 배열
var leftBarButtonItems: [UIBarButtonItem]? { get set }

// leftBarButtonItem : 좌측 아이템 영역의 UIBarButtonItem 중에 가장 좌측 바 버튼 아이템
var leftBarButtonItem: UIBarButtonItem? { get set }

// rightBarButtonItems : 우측 아이템 영역의 UIBarButtonItem의 바 버튼 아이템 배열
var rightBarButtonItems: [UIBarButtonItem]? { get set }

// rightBarButtonItem : 우측 아이템 영역의 UIBarButtonItem 중에 가증 우측 바 버튼 아이템
var rightBarButtonItem: UIBarButtonItem? { get set }
```


#### 메서드

```swift
// setLeftBarButtonItems(_:animated:) : 좌측 아이템 영역에 UIBarButtonItem 타입의 객체들을 순차적으로 설정하고 애니메이션 효과를 적용
func setLeftBarButtonItems(_ items: [UIBarButtonItem]?, animated: Bool)

// setLeftBarButton(_:animated:) : 좌측 아이템 영역에 UIBarButtonItem 타입의 객체를 설정하고 애니메이션 효과를 적용
func setLeftBarButton(_ item: UIBarButtonItem?, animated: Bool)

// setRightBarButtonItems(_:animated:) : 우측 내비게이션 아이템 영역에 UIBarButtonItem 타입의 객채들을 순차적으로 설정하고 애니메이션 효과를 적용
func setRightBarButtonItems(_ items: [UIBarButtonItem]?, animated: Bool)

//setRightBarButton(_:animated:) : 우측 아이템 영역에 UIBarButtonItem 타입의 객체를 설정하고 애니메이션 효과를 적용
func setRightBarButton(_ item: UIBarButtonItem?, animated: Bool)
```

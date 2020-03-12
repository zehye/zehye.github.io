---
layout: post
title: Scroll View
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Scroll View

스크롤뷰는 스크롤뷰 안에 포함된 뷰를 상,하,좌,우로 스크롤 할 수 있고 확대 및 축소할 수 있는 뷰를 의미한다.

그리고 스크롤뷰를 상속받아 활용되는 뷰로는 UITableView, UICollectionView, UITextView등 여러 UIKit 클래스가 있다.

<center>
<figure>
<img src="/assets/post-img/iOS/80.png" alt="" width="80%">
</figure>
</center>


### 스크롤뷰 상호작용

#### 주요 프로퍼티

delegate : 스크롤뷰 객체의 델리게이트

```swift
weak var delegate: UIScrollViewDelegate? { get set }​
```
UIScrollViewDelegate 프로토콜에 의해 선언된 메소드 델리게이트가 UIScrollView 클래스의 메시지에 응답


### 콘텐츠 크기 및 오프셋 관리

#### 주요 프로퍼티

contentSize : 콘텐츠뷰의 크기

```swift
var contentSize: CGSize { get set }
```

contentOffset : 콘텐츠뷰의 원점이 스크롤뷰의 원점에서 오프셋 된 지점

```swift
var contentOffset: CGPoint { get set }
```


#### 주요 메서드

```swift
// setContentOffset(_:animated:) : 스크롤뷰의 원점에 대한 콘텐츠뷰의 오프셋 설정
func setContentOffset(_ contentOffset: CGPoint, animated: Bool)
```

### 콘텐츠 삽입 동작 관리

#### 주요 프로퍼티

contentInset : 콘텐츠뷰와 안전 영역 또는 스크롤뷰 가장자리에 간격
```swift
var contentInset: UIEdgeInsets { get set }​
```

## 스크롤뷰 구성

#### 주요 프로퍼티

```swift
// isScrollEnabled : 스크롤링이 사용 가능한지 아닌지를 결정하는 부울 값
var isScrollEnabled: Bool { get set }​

// isDirectionalLockEnabled : 스크롤이 특정 방향으로 고정할지를 결정하는 부울 값
var isDirectionalLockEnabled: Bool { get set }

//isPagingEnabled : 스크롤뷰에서 페이징을 사용할 수 있는 여부를 결정하는 부울 값
var isPagingEnabled: Bool { get set }

//scrollsToTop :  스크롤 할 수 있는 제스처를 사용할지를 결정하는 부울 값
var scrollsToTop: Bool { get set }

// bounces : 스크롤뷰가 가장자리를 통과해서 다시 튀어나오는지 제어하는 부울 값
var bounces: Bool { get set }

// alwaysBounceVertical : 세로 스크롤이 콘텐츠뷰의 끝에 도달할 때 튀어 오르기가 항상 발생하는지를 결정하는 부울 값
var alwaysBounceVertical: Bool { get set }

// alwaysBounceHorizontal : 가로 스크롤이 콘텐츠뷰의 끝에 도달할 때 튀어 오르기가 항상 발생하는지를 결정하는 부울 값
var alwaysBounceHorizontal: Bool { get set }
```

### 스크롤링 상태 가져오기

#### 주요 프로퍼티

```swift
// isTracking : 사용자가 스크롤을 시작하기 위해 콘텐츠를 터치한 여부를 반환
var isTracking: Bool { get }

// isDragging : 사용자가 콘텐츠를 스크롤하고 있는지 나타내는 부울 값
var isDragging: Bool { get }

// isDecelerating : 사용자가 손가락을 떼었을 때 콘텐츠가 스크롤뷰에서 움직이지 않고 있는지를 반환
var isDecelerating: Bool { get }

// decelerationRate : 사용자가 손가락을 뗀 후의 감속도를 결정하는 부동 소수점 값
var decelerationRate: CGFloat { get set }
```

### 스크롤 인디케이터 및 새로고침 제어 관리

#### 주요 프로퍼티

```swift
// indicatorStyle : 스크롤 인디케이터의 스타일
var indicatorStyle: UIScrollViewIndicatorStyle { get set }

// showsHorizontalScrollIndicator : 가로 스크롤 바 표시 여부를 제어하는 부울 값
var showsHorizontalScrollIndicator: Bool { get set }

// showsVerticalScrollIndicator : 세로 스크롤 바 표시 여부를 제어하는 부울 값
var showsVerticalScrollIndicator: Bool { get set }
```

### 특정 위치로 스크롤 하기

#### 주요 메서드

```swift
// scrollRectToVisible(_:animated:) : 콘텐츠의 특정 위치로 스크롤 하여 화면에 표시
func scrollRectToVisible(_ rect: CGRect,  animated: Bool)
```


### 확대 및 축소

#### 주요 프로퍼티

```swift
// panGestureRecognizer : 팬 제스처를 제어하기 위한 제스처 인스턴스
var panGestureRecognizer: UIPanGestureRecognizer { get }

// pinchGestureRecognizer : 핀치 제스처를 제어하기 위한 제스처 인스턴스
var pinchGestureRecognizer: UIPinchGestureRecognizer? { get }

// zoomScale : 스크롤뷰 콘텐츠에 적용되는 현재 배율
var zoomScale: CGFloat { get set }

// maximumZoomScale :  스크롤뷰 콘텐츠에 적용되는 최대 배율
var maximumZoomScale: CGFloat { get set }

// minimumZoomScale : 스크롤뷰 콘텐츠에 적용되는 최소 배율
var minimumZoomScale: CGFloat { get set }

// isZoomBouncing : 확대 및 축소가 지정한 배율 제한을 초과했음을 나타내는 부울 값
var isZoomBouncing: Bool { get }

// isZooming : 콘텐츠뷰가 현재 확대 또는 축소되어 있는지를 나타내는 부울 값
var isZooming: Bool { get }

// bouncesZoom : 크기 조정이 최대 또는 최소 제한을 초과할 때 튀어 오르는 애니메이션을 보여줄지 결정하는 부울 값
var bouncesZoom: Bool { get set }
```

#### 주요 메서드

```swift
// zoom(to:animated:) : 콘텐츠 특정 영역 확대
func zoom(to rect: CGRect, animated: Bool)

// setZoomScale(_:animated:) : 현재 배율을 지정
func setZoomScale(_ scale: CGFloat, animated: Bool)
```

### 키보드 관리

#### 주요 프로퍼티

```swift
// keyboardDismissMode : 스크롤뷰에서 드래그가 시작될 때 키보드가 해제되는 방식
var keyboardDismissMode: UIScrollViewKeyboardDismissMode { get set }
```


## UIScrollViewDelegate 프로토콜

#### 스크롤 및 드래그

```swift
// scrollViewDidScroll(_:) : 콘텐츠뷰를 스크롤 할 때 델리게이트에 알림
optional func scrollViewDidScroll(_ scrollView: UIScrollView)

// scrollViewWillBeginDragging(_:) : 스크롤뷰에서 콘텐츠 스크롤을 시작할 시점을 델리게이트에 알림
optional func scrollViewWillBeginDragging(_ scrollView: UIScrollView)

// scrollViewWillEndDragging(_:withVelocity:targetContentOffset:) : 스크롤뷰의 드래그가 끝나기 직전에 델리게이트에 알림
optional func scrollViewWillEndDragging(_ scrollView: UIScrollView,
                        withVelocity velocity: CGPoint,
                 targetContentOffset: UnsafeMutablePointer<CGPoint>)

// scrollViewDidEndDragging(_:willDecelerate:) : 스크롤뷰의 드래그가 끝났을 때 델리게이트에 알림
optional func scrollViewDidEndDragging(_ scrollView: UIScrollView,
                     willDecelerate decelerate: Bool)

// scrollViewShouldScrollToTop(_:) : 스크롤뷰가 콘텐츠의 맨 위로 스크롤 해야 하는 경우 델리게이트에 동작 여부를 물어봄
optional func scrollViewShouldScrollToTop(_ scrollView: UIScrollView) -> Bool

// scrollViewDidScrollToTop(_:) : 스크롤뷰가 콘텐츠의 맨 위로 스크롤 되었음을 델리게이트에 알림
optional func scrollViewDidScrollToTop(_ scrollView: UIScrollView)

// scrollViewWillBeginDecelerating(_:) : 스크롤링 동작이 감속되기 시작하고 있다고 델리게이트에 알림
optional func scrollViewWillBeginDecelerating(_ scrollView: UIScrollView)

// scrollViewDidEndDecelerating(_:) : 스크롤링 동작이 감속이 끝났을 때 델리게이트에 알림

// optional func scrollViewDidEndDecelerating(_ scrollView: UIScrollView)
```

#### 확대 및 축소
```swift
//viewForZooming(in:) : 스크롤뷰에서 확대 및 축소를 할 때 확대 및 축소를 할 뷰 인스턴스를 요청
optional func viewForZooming(in scrollView: UIScrollView) -> UIView?

// scrollViewWillBeginZooming(_:with:) : 스크롤뷰의 콘텐츠 확대가 시작될 때 델리게이트에 알림
optional func scrollViewWillBeginZooming(_ scrollView: UIScrollView,
                                 with view: UIView?)

// scrollViewDidEndZooming(_:with:atScale:) : 스크롤뷰의 콘텐츠 확대가 완료될 때 델리게이트에 알림
optional func scrollViewDidEndZooming(_ scrollView: UIScrollView,
                              with view: UIView?,
                           atScale scale: CGFloat)

// scrollViewDidZoom(_:) : 스크롤뷰의 확대 및 축소 배율이 변경될 때 델리게이트에 알림
optional func scrollViewDidZoom(_ scrollView: UIScrollView)
```

#### 스크롤 애니메이션
```swift
scrollViewDidEndScrollingAnimation(_:) : 스크롤뷰의 스크롤 애니메이션이 끝날 때 델리게이트에 알림
optional func scrollViewDidEndScrollingAnimation(_ scrollView: UIScrollView)
```

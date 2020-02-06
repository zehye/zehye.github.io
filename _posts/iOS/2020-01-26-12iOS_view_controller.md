---
layout: post
title: UIViewController, 뷰의 상태변화 감지 메서드
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 뷰의 상태변화 감지 메서드

뷰가 화면에 보여지는 상태의 변화나 뷰의 레이아웃에 변화가 생기면 뷰 컨트롤러는 여러가지 메서드를 호출해 서브클래스가 적절한 대응을 할 수 있게 한다.

<center>
<figure>
<img src="/assets/post-img/iOS/15.png" alt="" width="80%">
</figure>
</center>

### 뷰의 상태 변화 메서드

뷰가 나타나거나 사라지는 등 뷰가 화면에 보이는 상태가 변화할때 호출되는 메서드이다.

- func viewDidLoad()
  - 뷰 계층이 메모리에 로드된 직후 호출되는 메서드 > 뷰의 추가적인 초기화 작업을 하기 좋은 시점
  - 메모리에 처음 로딩 될때 1회 호출되는 메서드로, 메모리 경고로 뷰가 사라지지 않는 이상 다시 호출되지 않음
- func viewWillAppear(_ animated: Bool)  
  - 뷰가 뷰 계층에 추가되고 화면이 표시되기 직전에 호출되는 메서드 > 뷰의 추가적인 초기화 작업을 하기 좋은 시점
  - 다른 뷰로 이동했다가 되돌아오면 재호출되는 메서드로, 화면이 나타날때마다 수행해야하는 작업을 하기 좋은 시점
- func viewDidAppear(_ animated: Bool)  
  - 뷰가 뷰 계층에 추가되어 화면이 표시되면 호출되는 메서드 > 뷰를 나타내는 것과 관련된 추가적인 작업을 하기 좋은 시점
- func viewWillDisappear(_ animated: Bool)
  - 뷰가 뷰 계층에서 사라지기 직전에 호출되는 메서드 >뷰가 생성된 뒤 발생한 변화를 이전상태로 되돌리기 좋은 시점
- func viewDidDisappear(_ animated: Bool)
  - 뷰가 뷰 계층에서 사라진 후 호출되는 메서드 > 뷰를 숨기는 것과 관련된 추가적인 작업을 하기 좋은 시점
  - 시간이 오래 걸리는 작업은 하지 않는 것이 좋음


### 뷰의 레이아웃 변화 메서드

뷰가 생성된 후 바운드 및 위치 등의 레이아웃에 변화가 발생했을 때 호출되는 메서드이다.

- func viewWillLayoutSubviews()
  - 뷰가 서브뷰의 레이아웃을 변경하기 직전에 호출되는 메서드 > 서브뷰의 레이아웃을 변경하기 전에 수행할 작업을 하기 좋은 시점
- func viewDidLayoutSubviews()
  - 서브뷰의 레이아웃이 변경된 후 호출되는 메서드 > 서브뷰의 레이아웃을 변경 한 후 추가적인 작업을 수행하기 좋은 시점

그 외에도 레이아웃 변경에 관련된 다양한 메서드가 있다. 자세한 내용은 UIViewController 클래스 소개 문서의 [Configuring the View’s Layout Behavior](https://developer.apple.com/documentation/uikit/uiviewcontroller) 부분을 참고

```swift
func viewLayoutMarginsDidChange()
func viewSafeAreaInsetsDidChange()
func updateViewConstraints()
// some more...
```
중요: 뷰 컨트롤러에서 위 메서드를 사용하기 위해 아래와 같이 override 키워드를 명시하고 super를 호출하는걸 잊지 않아야한다!

```swift
override func viewDidLoad() {
	super.viewDidLoad()
	// view가 메모리에 적재된 시점에서 필요한 코드 작성
}
```

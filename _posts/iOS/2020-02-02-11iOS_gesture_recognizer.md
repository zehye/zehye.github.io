---
layout: post
title: Gesture Recognizer
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Gesture Recognizer

제스처 인식기는 여러 제스처 관련 이벤트를 인식할 수 있다. <br>
특정 제스처 이벤트가 일어날 때 마다 각 타깃에 맞는 액션 메시지를 보내어 제스처 관련 이벤트를 처리할 수 있다.


### UIGestureRecognizer class

UIGestureRecognizer 클래스는 특정 제스처 인식기에 대한 동작을 정의하며 델리게이트 객체를 활용하여 일부 동작을 더욱 세밀하게 사용자화 할 수 있다.

#### UIGestureRecognizer의 하위 클래스

아래의 7가지의 UIGestureRecognizer 하위 클래스를 통해 여러 제스처를 인식할 수 있다.

- UITapGestureRecognizer : 싱글탭 또는 멀티탭 제스처
- UIPinchGestureRecognizer : 핀치(Pinch) 제스처
- UIRotationGestureRecognizer : 회전 제스처
- UISwipeGestureRecognizer : 스와이프(swipe) 제스처
- UIPanGestureRecognizer : 드래그(drag) 제스처
- UIScreenEdgePanGestureRecognizer : 화면 가장자리 드래그 제스처
- UILongPressGestureRecognizer : 롱프레스(long-press) 제스처


제스처 인식기를 사용하기 위해서 타깃-액션 연결을 설정한 후 UIView의 메서드인 `addGestureRecognizer(_:)` 메서드를 통해 뷰에 연결한다. 제스처가 인식되면 해당 제스처 이벤트에 연결된 타깃에 액션 메시지가 전달되고 호출되는 액션메서드는 아래의 메서드 구현 형식 중 하나와 같아야 한다.

```swift
@IBAction func myActionMethod()
@IBAction func myActionMethod(_ sender: UIGestureRecognizer)
```

윈도우는 뷰에 터치 이벤트를 전달하기 전에 뷰에 추가된 제스처 인식기에 터치 이벤트를 전달한다. 제스처 인식기가 터치 이벤트를 인식했을 경우 뷰는 터치 이벤트를 받지 못하고, 제스처 인식기가 터치 이벤트를 인식하지 못했을 경우 터치 이벤트를 뷰가 받게 되며 일반적인 제스처 인식기의 동작의 흐름은 **cancelsTouchesInView, delaysTouchesBegan, delaysTouchesEnded 프로퍼티의 값에 영향** 을 받는다.


#### UIGestureRecognizer의 주요 메서드

- init(target: Any?, action: Selector?) : 제스처 인식기를 타깃-액션의 연결을 통해 초기화
- func location(in: UIView?) -> CGPoint : 제스처가 발생한 좌표를 반환
- func addTarget(Any, action: Selector) : 제스처 인식기 객체에 타깃과 액션을 추가
- func removeTarget(Any?, action: Selector?) : 제스처 인식기 객체로부터 타깃과 액션을 제거
- func require(toFail: UIGestureRecognizer) : 여러 개의 제스처 인식기를 가지고 있을 때, 제스처 인식기 사이의 의존성을 설정


#### UIGestureRecognizer의 주요 프로퍼티

- var state: UIGestureRecognizerState : 현재 제스처 인식기의 상태를 나타냄
- var view: UIView? : 제스처 인식기가 연결된 뷰
- var isEnabled: Bool : 제스처 인식기가 사용 가능한 상태인지를 나타냄
- var cancelsTouchInView : 제스처가 인식되었을 때 터치 이벤트가 뷰로 전달되는 여부에 영향을 미침

이 프로퍼티가 **true(기본값)** 이고 제스처 인식기가 제스처를 인식했다면, 해당 제스처의 터치는 뷰로 전달되지 않는다. 이전에 전달된 터치들은 `touchesCancelled(_:with:)` 메시지를 통해 취소되고 제스처 인식기가 제스처를 인식 못하거나 이 프로퍼티의 값이 **false** 라면 뷰가 모든 터치를 전달받게 됩니다.

- var delaysTouchesBegan : began 단계에서 제스처 인식기가 추가된 뷰에 터치의 전달 지연 여부를 결정
- var delaysTouchesEnded : end 단계에서 제스처 인식기가 추가된 뷰에 터치의 전달 지연 여부를 결정




#### ViewController.swift

```swift
import UIKit

class ViewController: UIViewController {

    @IBAction func tapView(_ sender: UITapGestureRecognizer) {
        self.view.endEditing(true)
    }
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.

        let tapGesture: UITapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(self.tapView(_:)))

        self.view.addGestureRecognizer(tapGesture)
    }
}
```

혹은

```swift

import UIKit

class ViewController: UIViewController, UIGestureRecognizerDelegate {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        let tapGesture: UITapGestureRecognizer = UITapGestureRecognizer()
        tapGesture.delegate = self
        self.view.addGestureRecognizer(tapGesture)
    }

    func gestureRecognizer(_ gestureRecognizer: UIGestureRecognizer, shouldReceive touch: UITouch) -> Bool {
        self.view.endEditing(true)
        return true
    }
}
```

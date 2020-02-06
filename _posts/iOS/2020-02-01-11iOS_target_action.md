---
layout: post
title: Target-Action Pattern
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Target-Action 디자인 패턴

Target-Action 디자인 패턴은 iOS 환경에서 많이 사용하는 디자인 패턴 중 하나로 Target-Action 디자인 패턴에서 객체는 **이벤트가 발생할 때 다른 객체에 메시지를 보내는 데 필요한 정보를 포함** 한다.

- 액션은 특정 이벤트가 발생했을 때 호출할 메서드를 의미
- 타겟은 액션이 호출될 객체를 의미

이벤트 발생 시 전송된 메시지를 액션 메시지라고 하고, 타겟은 프레임워크 객체를 포함한 모든 객체가 될 수 있으나, 보통 컨트롤러가 되는 경우가 일반적이다.

1. 만약 특정 이벤트가 발생했을 때 abc라는 이름의 메서드를 호출해야 하는 상황이라고 생각해 보자.
2. 이 abc라는 (액션)메서드는 A라는 클래스에도 정의되어 있고, B라는 클래스에도 정의되어 있는 경우가 있다.
3. 이렇게 같은 메서드가 여러 클래스에 정의되어 있는 경우도 있고, 그런 클래스의 인스턴스가 여러개인 상황도 있다.
4. 이런 여러 가지 상황에서 우리가 원하는 객체를 Target으로 지정하면 abc라는 액션을 실행할 객체를 상황에 따라서 선택할 수 있다.

<center>
<figure>
<img src="/assets/post-img/iOS/21.png" alt="" width="80%">
</figure>
</center>

<center>
<figure>
<img src="/assets/post-img/iOS/22.png" alt="" width="80%">
</figure>
</center>


### 액션 메서드

액션 메서드는 특정한 양식이 필요하다.

IBAction은 인터페이스 빌더가 메서드를 인지할 수 있도록 해준다. <br>
스위프트 언어를 활용한 프로그래밍 방식에서 `@objc`는 Swift 클래스를 사용하는 Objective-C 코드가 있거나 Objective-C유형의 메서드를 사용하는 경우 필요하다.

```swift
// 프로그래밍 방식
@objc func doSomething(_ sender: Any) {

}
// 인터페이스 빌더
@IBAction func doSomething(_ sender: Any) {

}
```

> 아직까지 애플의 프레임워크는 Objective-C 언어로 작성된 코드가 많기 때문에 스위프트 언어로 작성한 코드에서는 Objective-C 코드와 호환하기 위해서 @objc라고 표시해주어야 한다.
스위프트 언어 4버전 이전의 컴파일러는 @objc를 자동으로 만들어 주었다. 하지만 이러한 방식은 자원 비용이 많이 들어 스위프트 4에서는 명시적으로 작성해야 한다.


### 컨트롤 이벤트

#### 컨트롤 이벤트와 액션과의 관계

UIKit에는 `UIButton`, `UISwitch`, `UIStepper` 등 `UIControl`을 상속받은 다양한 컨트롤 클래스가 있다. 그런 컨트롤 객체에 발생한 다양한 이벤트 종류를 특정 액션 메서드에 연결할 수 있는데 즉, **컨트롤 객체에서 특정 이벤트가 발생하면, 미리 지정해 둔 타겟의 액션을 호출하게 된다.**


#### 컨트롤 이벤트의 종류

컨트롤 이벤트는 `UIControlEvents`라는 타입으로 정의되어 있다.

- touchDown: 컨트롤을 터치했을 때 발생하는 이벤트 > UIControlEvents.touchDown
- touchDownRepeat: 컨트롤을 연속 터치 할 때 발생하는 이벤트 > UIControlEvents.touchDownRepeat
- touchDragInside: 컨트롤 범위 내에서 터치한 영역을 드래그 할 때 발생하는 이벤트 > UIControlEvents.touchDragInside
- touchDragOutside: 터치 영역이 컨트롤의 바깥쪽에서 드래그 할 때 발생하는 이벤트 > UIControlEvents.touchDragOutside
- touchDragEnter: 터치 영역이 컨트롤의 일정 영역 바깥쪽으로 나갔다가 다시 들어왔을 때 발생하는 이벤트 > UIControlEvents.touchDragEnter
- touchDragExit: 터치 영역이 컨트롤의 일정 영역 바깥쪽으로 나갔을 때 발생하는 이벤트 > UIControlEvents.touchDragExit
- touchUpInside: 컨트롤 영역 안쪽에서 터치 후 뗐을때 발생하는 이벤트 > UIControlEvents.touchUpInside
- touchUpOutside: 컨트롤 영역 안쪽에서 터치 후 컨트롤 밖에서 뗐을때 이벤트 > UIControlEvents.touchUpOutside
- touchCancel: 터치를 취소하는 이벤트 (touchUp 이벤트가 발생되지 않음) > UIControlEvents.touchCancel
- valueChanged: 터치를 드래그 및 다른 방법으로 조작하여 값이 변경되었을때 발생하는 이벤트 > UIControlEvents.valueChanged
- primaryActionTriggered: 버튼이 눌릴때 발생하는 이벤트 (iOS보다는 tvOS에서 사용) > UIControlEvents.primaryActionTriggered
- editingDidBegin: `UITextField`에서 편집이 시작될 때 호출되는 이벤트 > UIControlEvents.editingDidBegin
- editingChanged: `UITextField`에서 값이 바뀔 때마다 호출되는 이벤트 > UIControlEvents.editingChanged
- editingDidEnd: `UITextField`에서 외부객체와의 상호작용으로 인해 편집이 종료되었을 때 발생하는 이벤트 > UIControlEvents.editingDidEnd
- editingDidEndOnExit: `UITextField`의 편집상태에서 키보드의 `return` 키를 터치했을 때 발생하는 이벤트 > UIControlEvents.editingDidEndOnExit
- allTouchEvents: 모든 터치 이벤트 > UIControlEvents.allTouchEvents
- allEditingEvents: `UITextField`에서 편집작업의 이벤트 > UIControlEvents.allEditingEvents
- applicationReserved: 각각의 애플리케이션에서 프로그래머가 임의로 지정할 수 있는 이벤트 값의 범위 > UIControlEvents.applicationReserved
- systemReserved: 프레임워크 내에서 사용하는 예약된 이벤트 값의 범위 > UIControlEvents.systemReserved
- allEvents: 시스템 이벤트를 포함한 모든 이벤트 > UIControlEvents.allEvents




#### ViewController.swift

```swift
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var datePicker: UIDatePicker!
    @IBOutlet weak var dateLabel: UILabel!
    let dateFormatter: DateFormatter = {
        let formatter: DateFormatter = DateFormatter()
//        formatter.dateStyle = .medium
//        formatter.timeStyle = .medium
        formatter.dateFormat = "yyyy/MM/dd hh:mm:ss"
        return formatter
    }()

    @IBAction func didDatePickerValueChanged(_ sender: UIDatePicker) {
        print("value changed!")

        // 바꼇을때 date에 label을 표시(sender: 화면위에 올려놓은 datePicker)
        let date: Date = self.datePicker.date  // let date: Date = sender.date
        let dateString: String = self.dateFormatter.string(from: date)

        self.dateLabel.text = dateString
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.

        self.datePicker.addTarget(self, action: #selector(self.didDatePickerValueChanged(_:)), for: UIControl.Event.valueChanged)
    }
}
```

---
layout: post
title: 얼럿(alert)과 액션시트(action sheet)란 무엇인가?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.     

<hr>

## 얼럿과 액션시트

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/5.png" alt="" width="80%">
</figure>
</center>

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/6.png" alt="" width="80%">
</figure>
</center>


### UIAlertController 클래스

UIAlertController 클래스는 사용자에게 표시할 얼럿 또는 액션시트의 구성에 관한 메서드와 프로퍼티를 포함하고 있다. UIAlertController 클래스를 통해 얼럿 또는 액션시트를 구성한 후 UIViewController의 **present(_:animated:completion:)** 메서드를 사용하여 사용자에게 얼럿 또는 액션시트를 모달로 보여준다.


#### UIAlertController의 주요 메서드

- init(title:message:preferredStyle:) > 얼럿 뷰 컨트롤러의 객체를 초기화
- func addAction(UIAlertAction) > 얼럿이나 액션시트에 액션을 추가
- func addTextField(configurationHandler: ((UITextField) -> Void)? = nil) > 얼럿을 통해 텍스트를 입력받고자 하는 경우 텍스트 필드를 추가


#### UIAlertController의 주요 프로퍼티

- var title: String? > 얼럿의 제목
- var message: String? > 얼럿에 대해 좀 더 자세히 설명하는 텍스트
- var actions: [UIAlertAction] > 사용자가 얼럿 또는 액션시트에 응답하여 실행할 수 있는 액션
- var preferredStyle: UIAlertController.Style > 얼럿 컨트롤러의 스타일로 얼럿(alert)과 액션시트(actionSheet)가 있다.



### UIAlertAction 클래스

사용자가 얼럿 또는 액션시트에서 사용할 버튼과 버튼을 탭 했을 때 수행할 액션(작업)을 구성할 수 있다. UIAlertAction 클래스를 사용하여 버튼을 구성한 후 UIAlertController 객체에 추가하여 사용한다.


#### UIAlertAction의 주요 프로퍼티

- var title: String? > 액션 버튼의 타이틀
- var isEnabled: Bool > 액션이 현재 사용 가능한지를 나타냄
- var style: UIAlertAction.Style > 액션 버튼의 적용될 스타일


#### UIAlertAction.Style

- default: 액션 버튼의 기본 스타일
- cancel: 액션 작업을 취소하거나 상태 유지를 위해 변경사항이 없을 경우 적용하는 스타일
- destructive: 취하게 될 액션이 데이터를 변경되거나 삭제하여 돌이킬 수 없는 상황이 될 수 있음을 나타낼 때 사용하는 스타일

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/7.png" alt="" width="80%">
</figure>
</center>



## 얼럿과 액션시트는 언제 사용할까?

### 얼럿

- 중요한 액션을 하기 전 경고가 필요한 경우
- 액션을 취소할 기회를 제공해야 하는 경우
- 사용자의 작업을 한 번 더 확인하거나 삭제 등의 작업을 수행하거나 문제 사항을 알릴 때
- 결정이 필요한 중요 정보를 표시할 경우


### 액션시트

- 사용자가 고를 수 있는 액션 목록이 여러 개일 경우
- 새 작업 창을 열거나, 종료 여부 확인 시
- 사용자의 결정을 되돌리거나 그 동작이 중요하지 않을 경우



## 실습  

```swift
import UIKit

class ViewController: UIViewController {

    @IBAction func touchUpShowAlertBtn(_ sender: UIButton) {
        // 해당 버튼을 누르면 showAlertController를 실행하게 되고
        // 스타일을 alert 스타일로 해달라고 지정
        self.showAlertController(style: UIAlertController.Style.alert)
    }

    @IBAction func touchUpShowActionSheetBtn(_ sender: UIButton) {
        self.showAlertController(style: UIAlertController.Style.actionSheet)
    }

    func showAlertController(style: UIAlertController.Style) {
        let alertController: UIAlertController
        alertController = UIAlertController(title: "Title", message: "Message", preferredStyle: style)

        let okAction: UIAlertAction
        // handler는 alert action이 선택됐을때, OK버튼이 실행된다면
        // 실행될 코드 블럭을 의미한다
        okAction = UIAlertAction(title: "OK", style: UIAlertAction.Style.default, handler: { (action: UIAlertAction) in print("OK pressed")
        })

        let cancelAction: UIAlertAction
        // nil은 사용자가 누르면 아무 액션없이 alert이 dismiss된다
        cancelAction = UIAlertAction(title: "Cancel", style: UIAlertAction.Style.cancel, handler: nil)

        // action을 추가해줘야 실행된다.
        // 액션 순서에 상관없이 어떻게 넣어주더라도 위치는 UIAlertController가 알아서 지정해준다
        alertController.addAction(okAction)
        alertController.addAction(cancelAction)

        // 모달로 올려줌! completion은 모달이 올라오는 애니메이션이 끝나고 직후에 호출될 블럭
        self.present(alertController, animated: true, completion: { print("Alert controller shown") })
    }
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }
}
```

위 코드는 문제없이 진행될 것이고 이제 아래 코드를 더 추가해보자

```swift
let handler: (UIAlertAction) -> Void
        // handler라는 상수를 따로 빼놓음으로써 handler가 해야할 일을 지정해줌
        // 반복적으로 사용되는 작업은 이같이 사용하는 게 좋다
        // 어떤 응답에 대한 처리인지 써놔주었기 때문에 더 편리!
        handler = { (action: UIAlertAction) in print("action pressed \(action.title ?? "")")}

        let someAction: UIAlertAction
        someAction = UIAlertAction(title: "Some", style: UIAlertAction.Style.destructive, handler: handler)

        let anotherAction: UIAlertAction
        anotherAction = UIAlertAction(title: "Another", style: UIAlertAction.Style.cancel, handler: handler)

        alertController.addAction(someAction)
        alertController.addAction(anotherAction)
```

아래와 같은 에러가 뜰 것이다.

> Terminating app due to uncaught exception 'NSInternalInconsistencyException', reason: 'UIAlertController can only have one action with a style of UIAlertActionStyleCancel'

해당 에러는 다음과 같다. 현재 위 코드에서 액션 스타일 cancel을 두번 사용하고 있는데, 다른 스타일은 상관없지만 cancel은 하나의 액션만 스타일을 사용할 수 있다. 따라서 cancel 스타일은 하나만 남겨놓고 another action의 스타일을 default로 바꾸어주고 해보자!

```swift
let anotherAction: UIAlertAction
        anotherAction = UIAlertAction(title: "Another", style: UIAlertAction.Style.default, handler: handler)
```

완벽하게 진행될 것이다. 추가로,,

destructive 스타일이 쓰이는 떄는 삭제를 하거나 동작을 취했을때 혹은 되돌릴 수 없을때와 같이 주의해야하는 동작을 할때 사용하는 스타일이다.  

그리고 우리가 보통 앱스토어에서 로그인을 해야할때와 얼럿과 함께 텍스트필드를 사용하는 경우가 있다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/9.jpeg" alt="" width="80%">
</figure>
</center>

이는 아래와 같이 작성하면 된다.

```swift
alertController.addTextField(configurationHandler: { (field: UITextField) in
            field.placeholder = "플레이스 홀더"
            field.textColor = UIColor.red
        })
```

그런데 이 텍스트필드는 액션시트에서는 사용이 안되나보다...!

> Terminating app due to uncaught exception 'NSInternalInconsistencyException', reason: 'Text fields can only be added to an alert controller of style UIAlertControllerStyleAlert'

무언가 조금 아쉽....!

여튼! 이렇게 하면 아래와 같은 얼럿을 볼 수 있게 된다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/8.png" alt="" width="80%">
</figure>
</center>

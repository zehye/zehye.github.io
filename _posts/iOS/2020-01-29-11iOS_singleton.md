---
layout: post
title: Singleton Design Pattern과 Stack View
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 싱글턴 (SingleTon)

싱글턴은 '특정 클래스의 인스턴스가 오직 하나임을 보장하는 객체'를 의미한다.

싱글턴은 **애플리케이션이 요청한 횟수와는 관계없이 이미 생성된 같은 인스턴스를 반환** 한다. 즉, 애플리케이션 내에서 특정 클래스의 **인스턴스가 딱 하나** 만 있기 때문에 다른 인스턴스들이 공유해서 사용할 수 있다.

<center>
<figure>
<img src="/assets/post-img/iOS/17.png" alt="" width="80%">
</figure>
</center>


### Cocoa 프레임워크에서의 싱글턴 디자인 패턴

Cocoa 프레임워크에서 싱글턴 디자인 패턴을 활용하는 대표적인 클래스들이다.<br>
싱글턴 인스턴스를 반환하는 팩토리 메서드나 프로퍼티는 일반적으로 **shared라는 이름을 사용**

- [FileManager](https://developer.apple.com/documentation/foundation/filemanager)
  - 애플리케이션 파일 시스템을 관리하는 클래스 > FileManager.default
- [URLSession](https://developer.apple.com/documentation/foundation/urlsession)
  - URL 세션을 관리하는 클래스 > URLSession.shared
- [NotificationCenter](https://developer.apple.com/documentation/foundation/notificationcenter)
  - 등록된 알림의 정보를 사용할 수 있게 해주는 클래스 > NotificationCenter.default
- [UserDefaults](https://developer.apple.com/documentation/foundation/userdefaults)
  - Key-Value 형태로 간단한 데이터를 저장하고 관리할 수 있는 인터페이스를 제공하는 데이터베이스 클래스 > UserDefaults.standard
- [UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)
  - iOS에서 실행되는 중앙제어 애플리케이션 객체 > UIApplication.shared


##### 주의할 점

싱글턴 디자인 패턴은 애플리케이션 내의 특정 클래스의 인스턴스가 하나만 존재하기 때문에 객체가 불필요하게 여러 개 만들어질 필요가 없는 경우에 많이 사용한다. 예를 들면 환경설정, 네트워크 연결처리, 데이터 관리 등등이 있다. 하지만 멀티 스레드 환경에서 동시에 싱글턴 객체를 참조할 경우 원치 않은 결과를 가져올 수 있다. 어떤 디자인 패턴을 활용하더라도 항상 긍정적인 면과 위험성을 함께 고려하여 활용해야한다!


#### ViewController.swift

```swift
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var nameField: UITextField!
    @IBOutlet weak var ageField: UITextField!

    @IBAction func touchUpSetButton(_ sender: UIButton) {
        UserInformation.shared.name = nameField.text
        UserInformation.shared.age = ageField.text
    }
}
```

#### SecondViewController.swift

```swift
import UIKit

class SecondViewController: UIViewController {

    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var ageLabel: UILabel!


    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.
        print("SecondViewController의 view가 메모리에 로드됨")
    }

    override func viewDidAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        self.nameLabel.text = UserInformation.shared.name
        self.ageLabel.text = UserInformation.shared.age
        print("SecondViewController의 view가 화면에 보여짐")
    }
}
```

#### UserInformation.swift

```swift
import Foundation

class UserInformation {
    // 타입 프로퍼티를 통해 인스턴스를 하나 생성해 할당했기 때문에 이를 호출하면 항상 똑같은 인스턴스를 사용하게 될 것이다.
    static let shared: UserInformation = UserInformation()

    var name: String?
    var age: String?
}
``` 


## StackView

### UIStackView

스택뷰는 여러 뷰들의 수평 또는 수직 방향의 선형적인 레이아웃의 인터페이스를 사용할 수 있도록 해준다.

스택뷰와 오토레이아웃 기능을 활용하여 디바이스의 방향과 화면크기에 따라 동적으로 적응할 수 있는 사용자 인터페이스를 만들 수 있으며 스택뷰의 레이아웃은 스택뷰의 `axis`, `distribution`, `alignment`, `spacing`과 같은 프로퍼티를 통해 조정한다.


<center>
<figure>
<img src="/assets/post-img/iOS/19.png" alt="" width="80%">
</figure>
</center>



#### 수평(horizontal) axis 스택뷰 모식도

스택뷰는 `arrangedSubviews` 프로퍼티에 스택뷰에 포함된 뷰들을 관리한다. <br>
이 프로퍼티는 순서가 있는 배열과도 같으며, 스택뷰에 포함된 뷰의 순서가 레이아웃에 영향을 미친다.

<center>
<figure>
<img src="/assets/post-img/iOS/20.png" alt="" width="80%">
</figure>
</center>

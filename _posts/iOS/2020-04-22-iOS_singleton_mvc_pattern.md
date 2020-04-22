---
layout: post
title: 개발 디자인패턴(Singleton, MVC)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.     

<hr>


## 디자인패턴이란?

개발에서 디자인 패턴이란, 다양한 개발 환경에서도 비슷한 문제가 발생할 수 있는데 이 비슷한 문제들을 해결하는 정형화된 해결책을 의미한다. 즉, 개발에서 비슷한 문제를 해결하는 일종의 템플릿이나 개발패턴을 말한다고 생각하면 쉽다. 이 패턴을 적용해 코드의 가독성, 효율성, 디버깅, 협업 등이 쉬워지게 만들수 있다.


## Singleton 패턴

싱글턴 패턴은 어떤 클래스의 인스턴스가 단 하나만 존재하도록 강제하는 패턴을 의미한다. 앱 전체에서 해당 클래스의 인스턴스는 딱 하나만 만들어질 수 있게끔 설계하여 여러 객체들이 이를 참조하도록 하는 방식이다. 이는 데이터 자체가 하나의 원본으로 유지되어야 하거나, 인스턴스를 만드는 것이 리소스가 많이 드는 일이기 때문에 등 여러 이유로 사용되어지는 패턴이다.

```swift
UserDefault.standard  // 앱의 디폴트 설정상태를 정의, 저장하는 공간으로 하나의 앱에는 하나의 설정 인스턴스만 존재
UIApplication.shared  // 앱의 유저이벤트에 대응하는 라우팅을 담당하여 특성에 따라 하나의 인스턴스만 존재
UIScreen.main  // 유저가 들고있는 아이폰의 스크린자체를 의미하며 스크린이 하나니 하나의 인스턴스만 존재
FileManager.default  // 유저의 아이폰 하드드라이브에 저장된 파일들에 접근하는 것으로 하나의 인스턴스만 존재
```

싱글턴을 만들 때 가장 중요한 것은 인스턴스가 단 하나만 만들어질 수 있도록 확실하게 구조를 짜야한다는 것이다. 이를 위해서는 해당 인스턴스를 외부에서 새로 생성할 수 없도록 막아야 한다. init()을 private으로 선언해 외부에서 인스턴스를 만들 수 있는 여지를 완벽하게(원천적으로) 차단하는 방법이 있다.

```swift
class SingleTon {
  statie let shared = SingleTon()
  private init() {}
}
```

오직 타입프로퍼티인 shared를 통해서만 인스턴스를 활용할 수 있을 것이다.

## MVC 구조 패턴

MVC구조(Model-View-Controller)는 프로그램을 짜는 대표적인 디자인패턴 중 하나이다. MVC의 Model에는 앱의 비즈니스 로직이나 데이터를 담당하는 코드를 담아놓고, View에는 사용자에게 보여지는 모든 객체들을 다루는 코드를 담아놓으며, Controller에는 Model과 View 사이 중간자 역할을 하는 코드를 담아놓는다. 이러한 패턴을 MVC 패턴이라고 칭한다.

<center>
<figure>
<img src="/assets/post-img/swift/10.png" alt="" width="80%">
</figure>
</center>

이렇게 코드를 MVC 패턴으로 짜는 이유는 Model과 View 코드들을 각각 분리함으로써 범용성과 재사용성을 높이고, 각각의 역할을 분리하여 개발과 유지보수를 쉽게 하기 위함이다. Model과 View 코드는 분리되어야 하지 않는데 해당 코드들이 섞이기 시작한다면 해당 코드들의 범용성이 떨어져 다른곳에 재사용도 불가능할 뿐더러 코드 구조가 꼬여버려 분석이나 디버깅 및 확장이 어려운 코드가 되어버릴 위험이 커진다.


### Model

앱의 비즈니스 로직과 데이터를 관장하는 영역이다. 앱의 두뇌역을 하는 것으로 생각하면 쉬는데 계산기앱이라면 계산하는 로직과 관련된 데이터가, 채팅앱이라면 채팅 기능 관련 로직과 데이터가 Model 영역에 포함되게 된다. 서버로부터 데이터를 불러오거나 데이터 정합성을 관리하는 것도 Model의 역할이다.


### View

UI. 유저에게 데이터를 보여주거나, 유저 인터렉션(UI)을 담당하는 영역이다. UI와 UI로직을 담당한다고 보면 된다. '어떻게' 보여질지에 대해서만 관장하며 어떤 것을 보여줄 지에 대해서는 알지 못한다. 단순히 보여지는 영역을 어떻게 어떤 모양으로 보여줄지만 담겨있지 그 안의 세부적인 데이터나 무엇이 넘어오는지에 대해서는 View에서 알 수 없음을 의미한다. 즉, 자신의 Controller가 누군지, 어떤 데이터가 보여지는 지 등에 대해서는 알 수 없다.


### Controller

View와 Model을 이어주는 역할을 담당한다. View와 Model은 서로 독립적이어야 하기에 중간에서 Controller가 중간다리 역할을 해주게 된다. Controller는 View와 Model 사시에서 자유로이 소통을 하지만 반대로 View와 Model은 자신에게 연결되어있는 Controller가 누군지 알 수가 없다.



<hr>

### Controller to View: Outlet

Controller가 View에게 말을 걸기 위해서는 **outlet** 을 사용한다. 스토리보드에서 코드로 레이블이나 테이블뷰를 당겨오면 IBOutlet이 생기게 되는데 이게 바로 Controller가 View에게 말을 걸기 위해 해당 레이블이나 테이블 뷰 등의 인스턴스를 만드는 것을 의미한다. 스토리보드에서 코드영역으로 드래그를 하기에 View가 Controller에게 말을 거는 것처럼 보이겠지만 사실 그 반대이다. IB는 Interface Builder의 약자로 IBOutlet을 통해 Controller는 View에게 '너가 보여줄 이미지는 이것이다' 등과 같은 구체적인 지시를 하는 것으로 이해해보자.


```swift
@IBOutlet weak var nameLabel: UILabel!
@IBOutlet weak var ageTextField: UITextField!
```


### View to Controller

#### IBAction

View는 Controller의 꼭두각시일 뿐이지만 때에 따라 View가 Controller에게 말을 걸 수도 있다. 이는 슈퍼갑갑갑갑인 사용자가 View를 통해 메시지를 던질 수도 있기 때문이다. 간단하게 버튼을 생각하면 되는데 사용자가 바라보면 뷰에서 어떤 버튼을 눌러 이루어지는 메시지가 있을때 해당 버튼이 해야하는 일을 정해줘야 한다.

이를 처리하는 방법 중 하나가 Target-Action이다. Controller가 자신에게 과녁(Target)을 설정해놓고 View에게 그 과녁에 쏠 화살(Action)들을 건네주는 벙법이다.

```swift
@IBAction func nextBtn() {
  let storyboard = UIStoryboard.init(name: "Main", bundle: nil)
  let vc: SecondViewController = storyboard.instantiateViewController(withIdentifier: "SecondViewController") as! SecondViewController

  self.navigationController?.pushViewController(vc, animated: true)
}
```

버튼이 눌러져 Action이 발생하면 본 Controller의 nextBtn() 메서드가 Target으로 대응하게 될 것이다.


#### Delegation

두번째 방법은 Delegation Protocol을 사용하는 것이다. Controller class 안에 View class의 인스턴스를 만드는 IBOutlet처럼 View class 안에 Controller class를 만드는 것과 유사하다. 다만 class 대신 Protocol이 활용되는 것으로 Delegation은 View가 자신에 대한 제어권을 Controller에게 위임하는 protocol로서 앱을 사용하는 유저가 다음 5가지 행동을 하는 경우에 대한 권한이 위임된다.

- will
- did
- should
- data at
- count


### Controller to Model: 인스턴스

Model class의 인스턴스를 만든다.

### Model to Controller: Notification 및 KVO

- [Notification](https://www.zehye.kr/ios/2020/04/10/13iOS_notification/)
- [KVO](https://www.zehye.kr/ios/2020/03/19/11iOS_KVO/)

View가 실질적으로 Controller에게 말을 걸 수 없듯(다이렉트로) Model도 Controller에게 바로 말을 걸수는 없다. Model은 자신의 데이터가 변할때와 무엇인가 알려줄 필요가 있을 때, 라디오 스테이션처럼 자신의 주파수에 맞춘 Controller 및 Model들에게 관련 안내를 하는 구조로 교신을 하게 된다. 이러한 과정은 딕셔너리처럼 Key, Value를 관찰하는 Key-Value Observation(KVO)를 통해 수행되며 변경사항이 있을 경우 통지(Notification)가 가게 된다.



## MVC의 단점

### 1. Massive View Controller

모델에 넣기도 애매하고 뷰에 넣기도 애매한 코드들은 모두 컨트롤러에 들어가게 되다보니 컨트롤러가 비대해지게 된다.

### 2. View와 Controller가 너무 친하다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/28.png" alt="" width="50%">
</figure>
</center>

애플의 MVC 패턴은 기존 MVC패턴과는 조금 다르게 View와 Controller가 강학 연결되어 있어 ViewController가 거의 모든일을 다 한다. ViewController에서는 Controller가 View의 라이프사이클에 관여하기 때문에 View와 Controller를 분리하기 어렵다. 그래서 앱을 테스트할 때 Model은 따로 분리되어 테스트 할 수 있어도 View와 Controller는 강하게 연결되어 있어 각각 테스트하기 어렵다고 한다.




[참고한 블로그](http://blog.naver.com/PostView.nhn?blogId=jdub7138&logNo=220968244920&redirect=Dlog&widgetTypeCall=true)
[참고한 블로그](https://eunjin3786.tistory.com/31)

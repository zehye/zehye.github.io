---
layout: post
title: SnapKit에 대해 아는만큼 정리해보기 
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
공부하면서 더 추가해나갈 예정입니다. 틀린 부분 혹은 잘못된 부분이 있다면 알려주세요! :) 

<hr>

## SnapKit

SnapKit을 사용할 때는 해석 방향이 중요

```swift
make.top.equalTo(viewProgress.snp.bottom).offset(30)
```
순차적으로 해석 (만들다 > 상단을 > 똑같게 > viewProgress의 하단과 > offset은 30


### offset? inset?

- offset: element와의 간격에 사용
- inset: superView와의 간격에 사용

#### Q. childView가 superView로부터 top, bottom, left, right 50 spacing 각각 주려면 어떻게?

**1. offset: childView의 constraint = superView의 constraint + offset**

```swift 
self.box.make { (make) in 
  make.top.equalToSuperView().offset(50)
  make.left.equalToSuperView().offset(50)
  make.right.equalToSuperView().offset(-50)
  make.bottom.equalToSuperView().offset(-50)
}
```

**2. inset**

```swift 
self.box.make { (make) in
  make.top.left.bottom.right.equalToSuperView().inset(50)
}
```

위 코드는 아래와 같게 사용할 수도 있다.

```swift
self.box.make { (make) in
  make.edges.equalTo(UIEdgeInsets(top: 50, left: 50, bottom: 50, right: 50))
}
```
그치만 각각의 spacing이 같다면 위 방법이 더 낫다.


### left, right / leading, trailing 

기본적으로 leading, trailing을 사용하는게 localized 때문인데, 이와 같이 설정하면 right-to-left 순서로 읽는 지역에서는 화면이 거꾸로(flip)되어서 표시된다고 한다. 반면 left, right를 사용하면 안그럼!

- left, right는 모든 지역에서 같은 방향으로 나오지만 leading, trailing은 오른쪽에서 왼쪽으로 읽는 지역에서는 flip된 ui가 나오게 된다.
- text를 보여줄때에는 leading, trailing을 사용하는게 나을 것 같으며 그림, 지도와 같이 이미지가 보이는 view를 보여줄 때에는 left, right를 쓰는게 나을 것 같다

전체적인 예시를 한번 보자!

```swift
class mainVC: UIViewController {

  // 원하는 객채를 이름붙여 만들어줌 
  let nameLbl = UILabel()
  let nameTextField = UITextField()
  let changeBtn = UIButton()

  // 이름만 만들어준 객체를 실제로 뷰에 띄움 
  override func viewDidLoad() {
    super.viewDidLoad()
    self.view.addSubView(self.nameLbl)
    self.view.addSubView(self.nameTextField)
    self.view.addSubView(self.changeBtn)

    // 아래부터 띄워놓은 객체들에게 각각의 위치를 부여해줌
    self.nameLbl.snp.makeConstraints {
      // self.nameLbl은 superView로부터 center에 위치함 
      $0.center.equalToSuperView()
    }

    self.nameTextField.snp.makeConstraints {
      // self.nameTextField의 top은 superView로 부터 80 떨어짐
      // leading, trailing은 각각 24만큼 떨어짐 
      $0.top.equalToSuperView().offset(80)
      $0.leading.equalToSuperView().offset(24)
      $0.trailing.equalToSuperView().offset(-24)
  }

    self.changeBtn.snp.makeConstraints {
      // changeBtn의 top은 nameLabl의 bottom으로부터 24 떨어짐
      // leading, trailing은 nameLbl과 같게
      $0.top.equalTo(self.nameLbl.snp.bottom).offset(24)
      $0.leading.trailing.equalTo(self.nameLbl)
    }
  }
}
```


### translatesAutoresizingMaskIntoConstraints = false

코드로 constraints를 잡으려면 translatesAutoresizingMaskIntoConstraints = false를 명시해줘야하지만 <br>
snapKit은 translatesAutoresizingMaskIntoConstraints = false를 내부에서 알아서 해준다.

따라서 굳이 우리가 이를 명시해주지 않아도 되며, 아래는 snapKit의 LayoutConstraintItem 파일에 적혀있는 코드이다.

```swift 
extension LayoutConstraintItem {
  internal func prepare() {
    if let view = self as? ConstraintView {
      view.translatesAutoresizingMaskIntoConstraints = false
    }
  }
}
```


### Anchor

1. 모든 앵커와 제약 조건 자체를 함께 연결하는 것이 가능

```swift 
child.snp.makeContraints { make in 
  make.leading.top.trailing.bottom.equealToSuperView()
}
```

2. edges를 이용해 더욱 간편하게 사용 가능

```swift 
child.snp.makeConstraints { make in 
  make.edges.equalToSuperView()
}
```

3. view에 inset 값을 주고싶다면 inser() 가능

```swift 
child.snp.makeConstraints { make in 
  make.edges.equealToSuperView().inset(15)
}
```


### Constraints 

- multipliedBy()

```swift 
make.width.equalToSuperView().multipliedBy(0.45)
```
superView와 width를 같게 만들면서 0.45를 곱한 상태 


### 암시

constraints 기준이 되는 view 값을 최대한 짧게 작성한다.<br>
아래 코드는 다 같은 의미를 지닌다.

```swift 
view.snp.makeConstraints { make in 
  make.width.equalTo(otherView.snp.width
  make.centerX.equalTo(otherView.snp.centerX)
}

view.snp.makeConstraints { make in 
  make.width.equalTo(otherView)
  make.centerX.equalTo(otherView)
}

view.snp.makeConstraints { make in 
  make.width.centerX.equalTo(otherView)
}
```


### UpdateConstraints

constraints 기준이 될 기존에 지정한 view가 바뀌는게 아닌 constant value(상수값)만 업데이트

```swift 
extension QuizVC {
  override func willTransition (
    to newCollection: UITraitCollection,
    with coordinator: UIViewControllerTransitionCOordinator
  ) {
    super.willTransition(to: newCollection, with: coordinator)

    // 현재 방향을 설정(bool)
    let isPortrait = UIDevice.current.orientation.isPortrait

    // 세로일 경우 45, 가로일 경우 65로 높이를 바꿔줌
    lblTimer.snp.updateConstraints { make in 
        make.height.equalTo(isPortrait ? 45 : 65)
    }

    // 글꼴 크기를 변경해줌 
    lblTimer.font = UIFont.systerFont(ofize: isPortrait ? 20 : 32, weight: .light)
  }
} 
```


### RemakeConstraints

constant value만 변경되는 것이 아닌 기준이 될 view도 업데이트 <br>
기존에 존재하던 constraints는 삭제된다.

```swift 
func updateConstraints {
  pView.snp.remakeConstraints { make in 
    make.top.equalTo(view.safeAreaLayoutGuide)
    make.width.equalToSuperView().multipliedby(0.1)
    make.height.equalTo(32)
    make.leading.equalToSuperView()
  }
}
```


### lessThanOrEqualTo, priority > 아직 공부 더 필요

```swift 
override func updateConstaints() {
  self.prowingBtn.snp.updateConstraints { make in
    make.center.equalTo(self);
    make.width.height.equalTo(buttonSize).priority(250)
    make.width.height.lessThanOrEqualTo(self)
  }
  super.updateConstraints()
}
```

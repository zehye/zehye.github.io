---
layout: post
title: Stanford Lecture1 - Introduction to iOS11, Xcode9 and Swift4
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
Stanford Developing iOS11 Apps with Swift를 듣고 정리하였습니다.

<hr>


## What's in iOS?

iOS는 네개의 층으로 구성되어 있다. 각 층은 **Cocoa Touch**, Media, **Core Services**, Core OS이다.<br>
맨 아래층은 하드웨어에 가깝고, 맨 위층은 사용자에 가깝다.

- Cocoa Touch: 화면이나 버튼 등 유저 인터페이스(UI)와 관련된 layer
- Media: 음악, 영상, 애니메이션, 사진 등과 관련된 layer
- Core Services: Foundation이 포함된 layer로 네트워킹, 데이터베이스, 파일관리와 관련된 layer
- Core OS: C언어로 짜여진 유닉스 OS layer로 앱 개발자로서 거의 직접 접근하지 않는 layer


### Xcode 초기 설정

<center>
<figure>
<img src="/assets/post-img/swift/1.jpeg" alt="" width="80%">
</figure>
</center>


#### Main.storyboard

UI. xcode를 통해 UI를 그래픽으로 구현 -> **코드를 작성할 필요없이 그래픽으로 구현** <br>
버튼, 텍스트필드, 슬라이드 등을 드래그하여 사용하고 버튼과 슬라이더가 스크린에 실시간으로 나타나 원하는대로 편집하고 실행하면 됨


#### ViewContrller.swift

Main.storyboard에서 만든 **UI와 코드를 연결** 하는 곳<br>
UIKit는 버튼과 슬라이더 등이 있는 iOS 프레임워크 -> 코코아터치 층 같은 것과 비슷

```swift
import UIKit

class ViewController: UIViewController {
  // UIViewController는 UIKit에 들어있고 SuperClass로 ViewContrller는 UIViewController로부터 상속을 받음으로써 제어하는 모든기능을 상속받음
}
```


### UIButton

<center>
<figure>
<img src="/assets/post-img/swift/6.jpeg" alt="" width="50%">
</figure>
</center>


#### connection:action

<center>
<figure>
<img src="/assets/post-img/swift/2.jpeg" alt="" width="50%">
</figure>
</center>

action: 버튼을 눌렀을 때 메서드를 호출하라는 의미

이 메서드는 인수(arguments)를 가질수도 있고 가지지 않을 수도 있다. <br>
**None** 은 인수가 없어도 괜찮은 것이고 이외에는 **sender** 로 설정. 이 경우에는 UIKit를 보내주는 인수를 의미함으로 인수는 꼭 필요하다.

즉, touchCard가 넘어오면 뒤집을 수 있어야 하는데 이러한 소통을 위해 인수가 존재한다.

Type은 인수의 타입으로 Any가 되어있는데 `Button`으로 변경해줘야 한다.<br>
버튼이 메소드를 보내주는 것이기 때문으로 만약 `UIButton`으로 변경해주지 않으면 나머지 코드가 작동하지 않는다.

event는 Touch Up Inside로 경계안에서 터치하고 손을 뗄 때 이 메시지를 보내라는 뜻을 가진다.


#### IBAction와 _ 의미

<center>
<figure>
<img src="/assets/post-img/swift/3.jpeg" alt="" width="50%">
</figure>
</center>


13번째 줄이 원으로 바뀌었는데, 원에 마우스를 올려보면 어떤 버튼이 메소드를 부르는지 보여준다.

swift의 모든 인수에는 이름이 있으며 메소드를 부를때 이 이름을 포함해야 한다. 즉, 메소드를 호출하거나 코드를 읽을 때 첫번째 인수가 무엇인지 기억할 필요가 없다.
그리고 이 인수의 이름은 두개로 하나는 호출할 때 사용하는 **외부이름** 이고 하나는 우리가 코드 구현에 사용할 **내부 이름** 이다.

이름을 하나만 가지는 것도 가능하고 유효하다. withemoji가 emoji가 된다면 외부, 내부 이름 모두 emoji가 된다.<br>
그리고 만약 인수 앞에 `_` 가 있다면 그것은 인수가 없다는 것을 의미한다. (거의 사용하지는 않는다.)

```swift
func flipCard(withEmoji emoji:String, on button: UIButton) {}
```

touchCard에는 _ 가 있는데 iOS에서 메시지를 보내는 것으로 이건 Objective-C에서 왔고 여기서는 내부 외부 이름이 없었기 때문에 있다.

```swift
@IBAction func touchCard(_ sender: UIButton)
```

근본적으로 flipCard는 토글이다. flipCard 메소드에게 버튼을 확인해서 이미 유령이면 텍스트 없이 주황색으로 뒤집게 한다. <br>
아니라면 흰 바탕에 유령이 있게 하면 된다. 최소한의 값은 **normal state** 로 설정해야한다.

<center>
<figure>
<img src="/assets/post-img/swift/4.jpeg" alt="" width="50%">
</figure>
</center>


```swift
import UIKit

class ViewController: UIViewController {
  func flipCard(withEmoji emoji:String, on button: UIButton) {
  //        print("flipCard(withEmoji: \(emoji))")
      // 버튼의 현재 타이틀이 유령으로 되어있는지를 확인
      if button.currentTitle == emoji {
          button.setTitle("", for: UIControl.State.normal)
          button.backgroundColor = #colorLiteral(red: 1, green: 0.5763723254, blue: 0, alpha: 1)
      } else { // 유령이 없다면 유령을 넣어줘야 함
          button.setTitle(emoji, for: UIControl.State.normal)
          button.backgroundColor = #colorLiteral(red: 1, green: 1, blue: 1, alpha: 1)
      }
  }
}
```


#### 에러: swift는 모든 인스턴스 변수(속성)은 초기화를 해야한다. 즉, 속성엔 초기값이 항상 있어야 한다.

<center>
<figure>
<img src="/assets/post-img/swift/7.png" alt="" width="50%">
</figure>
</center>
<center>
<figure>
<img src="/assets/post-img/swift/5.png" alt="" width="50%">
</figure>
</center>


초기화 방법에는 두가지가 있다.

1. 이니셜라이저를 이용.
- 이니셜라이저는 단지 특별한 이름을 가진 메소드일 뿐이다. -> `init`
- init은 어떤 인수든 다 가질 수 있고 다른 인수를 가진 여러개의 init을 만드는것도 가능하지만 각 init은 모든 변수를 초기화한다.
2. 값에 0을 쓰는것


#### swift의 타입 지정

swift는 강한 타입 추론을 가지는 언어로 어떤 타입을 사용하는지 분명히 해야하며, 가능하기만 하다면 타입을 추측해준다.


### UILabel

읽기전용 텍스트 필드


#### connection:outlet

outlet: 인스턴스 변수(속성)를 만든다.<br>
이 속성은 UILabel을 가리키고 횟수가 바뀌면 스스로를 업데이트 하라고 말해준다.

```swift
@IBOutlet weak var flipCountLabel: UILabel!
```

`!`는 일단 굳이 초기화를 하지 않아도 된다는 특징이 있다. 초기화를 하지않아도 에러가 뜨지 않는다.

모든 속성을 원한다면 속성 다음 `didSet`이라는 코드를 추가할 수 있다.

```swift
var flipCount = 0 {
     didSet {
         flipCountLabel.text = "Flpis: \(flipCount)"
     }
 }
```

그러면 이 속성이 설정될때마다 이 안의 코드가 실행된다.<br>
이를 **속성감시자** 라고 하고 이 코드가 변화를 감시하고 있기 때문에 해당 코드를 꺼내 붙이고 없앨 수도 있다.

즉, filpCount가 바뀔때마다 didSet을 실행하고 레이블을 통해 업데이트를 할 것이다.<br>
UI와 인스턴스 변수의 싱크를 맞추기 위해 속성감시자는 많이 사용된다


#### 배열

모든 카드의 배열을 만든다.

- touchCard가 눌리면 배열을 보고 눌러진 버튼을 찾아내면 어떤 카드인지 알 수 있다.
- 어떤 카드가 배열 안 어느 인덱스에 있는지 알면 다른 배열에서 사용할 이모티콘을 찾는다

데이터에 의해 구동되는 것으로 카드를 추가하고 싶다면 이모티콘 배열에 카드를 추가하기만 하면 된다.


#### 카드를 담을 배열을 어떻게 만드는가?

UI와 코드의 연결이기 때문에 드래그로! var를 하나 더 만든다.


#### connection:Outlet Collection

UI에 잇는 것들의 배열

```swift
@IBOutlet var cardButtons: [UIButton]!
```

ULButton의 배열이라는 뜻의 특별한 문법이다.


### optional

index의 리턴값은 int가 아니다. optional 값이다. 물음표가 그 뜻이고 ?는 optional이라는 뜻이다.
optional과 int는 전혀 다른 타입이고 int와는 아무런 상관이 없다

optional은 오직 두가지 상태를 가지는 타입으로 설정된 것과 설정되지 않은 상태 => 열거형
열거형은 다른 언어와 마찬가지로 불연속형 값들의 모음이다
열거형의 각 경우마다 관련된 데이터를 가질 수 있다
같이 따라다니는 데이터
optional은 설정된 상태일 때 관련된 데이터가 있는데 이 경우 int인 것을 의미한다

이 Index 메소드는 주어진 버튼을 찾을 수 있는 지 없는지를 설정되었거나 되지 않은 상태로써 리턴하는 것

그리고 만약 찾앗다면 그것과 관련된 int형 데이터도 리턴한다.
cardNumber를 출력하면 그건 optional이고 설정된 상태이며 관련된 갑은 int라고 알려주는 것
cardNumber배열에 없는 카드를 클릭하면 nil이라고 나온다.
swift에서 nil은 설정되지 않은 optional의 상태를 의미한다

여기서는 optional값은 옳지않다(이모티콘 배열에서 옵셔널로 찾아볼 수는 없다.) 정확한 Int값이 필요하다.
그러면 설정된 상태의 관련된 값은 어떻게 가져올까? = 끝에 느낌표를 붙이는 것도 하나의 방법이다
느낌표를 붙이면 optional이 설정되었다고 가정하고 관련된 값을 가져오라는 뜻이다.

let cardNumber = cardButtons.lastIndex(of: sender)!

optional 문법은 정말 간단하게 ? ! 이런식으로 되어있는데, 이는 optional이 굉장히 흔히 쓰이기 때문이다.

이때 연결되지 않은 애를 클릭하면 충돌이 일어나고
이 충돌이 일어나는 이유는 설정되지 않은 옵셔널을 리턴하기 때문이다
관련된 값이 없으니 프로그램을 충돌시킨다.


예상하지 못한 nil을 발견했다는 뜻으로 optional을 푸는 과정에서 발생했다고 적혀잇음


그러나 이렇게 충돌이 일어나지 않는 방법을 쓰고 싶은 경우도 있다.
조건적으로 설정된 상태에 있는 지 보고 맞으면 쓰고 아니면 쓰지 않도록 한다.

그럴때에는 !가 아닌 앞에 if를 적는다.

if let cardNumber = cardButtons.lastIndex(of: sender)

이제 이 optional이 설정된 상태에 있다면 코드가 실행되고 아니라면 실행되지 않는다.
충돌은 일어나지 않는다.

여기서도 optional은 제일 간단한 문법을 가진다는 걸 알 수 있다.



!: 초기화하지 않아도 되게 해준다.
optional이긴 하지만 ?가 아니라 !라 약간 다른 optional이다.


마지막으로 받은 cardNumber를 이모티콘 배열에서 찾아보는 것


UI와 코드 모두 건드리는 것의 이름을 바꿀때는 cmd+rename

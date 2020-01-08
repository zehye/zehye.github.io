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


### 카드만들기 - UIButton 써보기

<center>
<figure>
<img src="/assets/post-img/swift/6.jpeg" alt="" width="50%">
</figure>
</center>

카드를 구성하기 위해 Button이라는 객체를 사용하게 된다. 이 Button은 View를 상속받고 있기 때문에 배경화면을 설정할 수 있다.


### UI에 있는 항목 조작하기

UI에 있는 항목을 조작하기 위해서는 실행에 대해 코드를 연결해야 한다.

<center>
<figure>
<img src="/assets/post-img/swift/2.jpeg" alt="" width="50%">
</figure>
</center>

action: 버튼을 눌렀을 때 메서드를 호출하라는 의미

이 메서드는 인수(arguments)를 가질수도 있고 가지지 않을 수도 있다. <br>
**None** 은 인수가 없어도 괜찮은 것이고 이외에는 **sender** 로 설정. 이 경우에는 UIKit를 보내주는 인수를 의미함으로 인수는 꼭 필요하다.

즉, touchCard가 넘어오면 뒤집을 수 있어야 하는데 이러한 소통을 위해 인수가 존재한다.

Type은 인수의 타입으로 Any가 되어있는데 Button으로 변경해줘야 한다.<br>
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

touchCard에는 _ 가 있는데 iOS에서 메시지를 보내는 것으로 이건 Objective-C에서 왔고 여기서는 내부 외부 이름이 없었기 때문에 있다.

```swift
@IBAction func touchCard(_ sender: UIButton)
```


### 카드뒤집기

카드를 클릭했을 때 카드가 뒤집히는 효과를 주기 위해 함수를 하나 선언한다. 두개의 인자를 받는 함수이며, 개별 인자에 대해 '외부이름'과 '내부이름'을 설정한다. 실제 Button을 클릭했을때, flipCard라는 함수가 호출되고 해당 함수에 두가지 인자를 전달하게 된다. 첫번째 인자는 고스트 이모지이며, 두번째 인자는 눌러진 버튼이다. 이렇게 flipCard로 전달된 두개의 인자를 이용해 우리가 원하는 결과(카드 뒤집기)를 만들어낸다.

```swift
func flipCard(withEmoji emoji:String, on button: UIButton) {}
```

근본적으로 flipCard는 토글이다. flipCard 메소드에게 버튼을 확인해서 이미 유령이면 텍스트 없이 주황색으로 뒤집게 한다. <br>
아니라면 흰 바탕에 유령이 있게 하면 된다. 최소한의 값은 **normal state** 로 설정해야한다.

<center>
<figure>
<img src="/assets/post-img/swift/4.jpeg" alt="" width="50%">
</figure>
</center>


```swift
func flipCard(withEmoji emoji:String, on button: UIButton) {
    // print("flipCard(withEmoji: \(emoji))")

    // 버튼의 현재 타이틀이 유령으로 되어있는지를 확인
    if button.currentTitle == emoji {
        button.setTitle("", for: UIControl.State.normal)
        button.backgroundColor = #colorLiteral(red: 1, green: 0.5763723254, blue: 0, alpha: 1)
    } else { // 유령이 없다면 유령을 넣어줘야 함
        button.setTitle(emoji, for: UIControl.State.normal)
        button.backgroundColor = #colorLiteral(red: 1, green: 1, blue: 1, alpha: 1)
    }
}
```
button.currentTitle에 따라 다른내용을 수행하게 한다.

> 토글? 하나의 설정값으로부터 다른 값으로 전환하는것으로 오직 두가지 상태밖에 없는 상황에서 스위치를 한번 누르면 한 값이 되고, 다시 한번 누르면 다른 값으로 변하는 것을 의미한다.


### 몇번 뒤집는지 확인해보기

<center>
<figure>
<img src="/assets/post-img/swift/5.png" alt="" width="50%">
</figure>
</center>

이와 같은 방식으로 선언을 하게 되면 에러를 만나게 된다.


#### swift는 모든 인스턴스 변수(속성)은 초기화를 해야한다. 즉, 속성엔 초기값이 항상 있어야 한다.

<center>
<figure>
<img src="/assets/post-img/swift/7.png" alt="" width="50%">
</figure>
</center>

초기화 방법에는 두가지가 있다.

1. 이니셜라이저를 이용.
- 이니셜라이저는 단지 특별한 이름을 가진 메소드일 뿐이다. -> `init`
- init은 어떤 인수든 다 가질 수 있고 다른 인수를 가진 여러개의 init을 만드는것도 가능하지만 각 init은 모든 변수를 초기화한다.
2. 값에 0을 쓰는것


```swift
var flipCount = 0
```

위와 같이 0을 할당함으로써 간단하게 초기화하도록 한다. swift의 경우 타입에 매우 엄격하다. 사용되는 거의 모든 변수는 타입을 가지며, Objective-C와의 호환을 위해 타입이 없는 경우가 종종있을 뿐이다. 변수의 타입을 명시적으로 적을수도 있지만, 특정 값을 할당하게 되면 해당 변수의 타입을 추론하기도 한다.



### 다른 카드 만들기 - UILabel 써보기

UILabel: 읽기전용 텍스트 필드

outlet: 인스턴스 변수(속성)를 만든다.<br>
이 속성은 UILabel을 가리키고 횟수가 바뀌면 스스로를 업데이트 하라고 말해준다.

```swift
@IBOutlet weak var flipCountLabel: UILabel!
```

`!`는 일단 굳이 초기화를 하지 않아도 된다는 특징이 있다. 초기화를 하지않아도 에러가 뜨지 않는다.

모든 속성을 원한다면 속성 다음 `didSet`이라는 코드를 추가할 수 있다. didSet을 사용하게 되면 매번 flipCountLabel의 텍스트 값을 할당하는 코드를 넣어주지 않고도 원하는 결과값을 얻을 수 있게 된다. 프로퍼티의 값이 바뀐 후에 실행되는 프로퍼티 감시다(didSet)이다.

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


### 여러장의 카드 - 배열

여러장의 카드를 만들기 위해서 버튼들을 배열에 넣고 인덱스를 통해 조작하도록 하면 새롭게 생기게 되는 모든 카드에 대해 반복적으로 함수를 선언할 필요가 없다.

UI와 코드의 연결이기 때문에 드래그로! var를 하나 더 만든다.

connection:Outlet Collection > UI에 잇는 것들의 배열(ULButton의 배열이라는 뜻의 특별한 문법이다)

```swift
@IBOutlet var cardButtons: [UIButton]!
```

### optional

여러장의 카드를 만들기 위해 배열을 만들엇다면 이 배열속에 저장된 버튼의 인덱스 값을 접근하기 위해 아래와 같은 코드를 작성한다.

```swift
let cardNumber = cardButtons.lastIndex(of: sender)
```

이때 인덱스에 Option+click을 하게 되면 반환값이 Int가 아닌 `Int?`임을 알 수 있다.

<center>
<figure>
<img src="/assets/post-img/swift/8.png" alt="" width="50%">
</figure>
</center>

index의 리턴값은 optional 값이다. **물음표가 그 뜻이고 ?는 optional이라는 뜻이다.** <br>
optional과 int는 전혀 다른 타입이고 int와는 아무런 상관이 없다

optional은 값이 있거나 없는 두가지 경우만 존재한다. optional 타입인 변수에 값이 할당되지 않은 상태에서 값이 있다고 변수를 사용하게 되면 충돌이 일어나게 된다.

<center>
<figure>
<img src="/assets/post-img/swift/9.png" alt="" width="50%">
</figure>
</center>

예상하지 못한 nil을 발견했다는 뜻으로 optional을 푸는 과정에서 발생했다는 의미의 에러이다. 이 optional을 정상적으로 사용하기 위한 방법으로 해당 타입의 뒤에 `느낌표(!)`를 붙이거나 `if let` 조건문을 활용하는 방법이 있다.

optional 문법은 정말 간단하게 ? ! 이런식으로 되어있는데, 이는 optional이 굉장히 흔히 쓰이기 때문이다.


#### 충돌을 일으키는 느낌표(!)

```swift
let cardNumber = cardButtons.lastIndex(of: sender)!
```

느낌표를 사용하는 경우 연결되지 않은 애를 클릭하면 충돌이 일어나고 이 충돌이 일어나는 이유는 설정되지 않은 optional을 리턴하기 때문이다. 즉 관련된 값이 없으니 프로그램을 충돌시킨다.


그러나 이렇게 충돌이 일어나지 않는 방법을 쓰고 싶은 경우도 있다.

#### 충돌을 일으키지 않는 if let 조건문

조건적으로 설정된 상태에 있는 지 보고 맞으면 쓰고 아니면 쓰지 않도록 한다.

```swift
if let cardNumber = cardButtons.lastIndex(of: sender)
```

이제 이 optional이 설정된 상태에 있다면 코드가 실행되고 아니라면 실행되지 않는다.충돌은 일어나지 않는다.


### cardNumber를 이모티콘 배열에서 찾아보기

```swift
var emojiChoices = ["👻","🎃","👻","🎃"]
```


## 전체 소스코드

```swift
import UIKit

class ViewController: UIViewController {

    var flipCount = 0 {
        didSet {
            flipCountLabel.text = "Flpis: \(flipCount)"
        }
    }

    @IBOutlet var cardButtons: [UIButton]!

    @IBOutlet weak var flipCountLabel: UILabel!

    var emojiChoices = ["👻","🎃","👻","🎃"]

    @IBAction func touchCard(_ sender: UIButton) {
        flipCount += 1
//        flipCountLabel.text = "Flpis: \(flipCount)"
//        flipCard(withEmoji: "👻", on: sender)
        if let cardNumber = cardButtons.lastIndex(of: sender) {
            flipCard(withEmoji: emojiChoices[cardNumber], on: sender)
            print("cardNumber: \(cardNumber)")
        } else {
            print("chosen card was not in cardButtons")
        }
    }


    func flipCard(withEmoji emoji:String, on button: UIButton) {
        // print("flipCard(withEmoji: \(emoji))")
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


- 참고: UI와 코드 모두 건드리는 것의 이름을 바꿀때는 cmd+rename

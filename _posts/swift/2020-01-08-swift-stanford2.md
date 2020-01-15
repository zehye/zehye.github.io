---
layout: post
title: Stanford Lecture2 - MVC(Model-View-Control)패턴
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
Stanford Developing iOS11 Apps with Swift를 듣고 정리하였습니다.

<hr>

## MVC 디자인 패러다임

일단 기본적으로 우리 시스템 안의 모든 객체는 세가지로 나뉜다. > **Model, View, Controller**


#### 1. Model

앱에서 '무엇'에 해당하는 UI와 독립적인 객체들이다. 집중력게임에서는 집중력게임을 할 줄 아는 부분에 해당하는 앱이다.
카드가 매치되는지 확인하고, 가져가며 언제 카드를 뒤집어야 하는지 그런 것들을 아는 부분이다. 즉, 집중력 게임에 대한 지식들을 의미한다.

하지만 그게 어떻게 화면에 나오지는 지에 대한 부분은 없다.


#### 2. Controller

'어떻게'에 해당한다. 어떻게 집중력 게임이 화면에 나오는지에 관심을 갖는다.<br>
Model의 정보를 해석하고 구성해서 View에게 보내주는 역할 혹은 사용자와의 상호작용을 모델에게 해석해주는 역할


#### 3. View

컨트롤러의 하인들로 보통 아주 일반적인 UI 요소들인데(UIButton, UILabel, UIViewController) 컨트롤러가 모델과 통신해 앱에서 어떤 것을 UI에 가져오도록 할 때 필요하다.


이러한 MVC는 세가지 캠프 사이의 커뮤니케이션을 관리하는 데에 의미가 있다. 캠프에 객체를 넣으면 서로 얘기할 때 특정한 규칙을 따라야 한다.


## MVC 서로의 관계

<center>
<figure>
<img src="/assets/post-img/swift/10.jpeg" alt="" width="80%">
</figure>
</center>


### Model과 Controller

**Controller > Model**: Controller가 원하는 대로 Model과 이야기 할 수 있다.
- 무엇이 어떤건지에 대해 사용자에게 보여줘야 하기 때문에 모델에 대한 접근이 가능해야 한다.
- 모델의 공개된 모든 기능과는 거의 무제한적인 대화가 가능하다.

**Model > Controller**: 직접적인 소통은 불가능하다.
- Model은 UI와 독립적이고 Controller는 근본적으로 UI에 종속된다.

그러나 소통은 가능하다. Model의 데이터가 변경되었을때 해당 변경사항과 연결된 UI에도 업데이트를 하는 경우!

`Notification & KVO(key value observing)`: Model의 변경사항을 방송하는 것

### Model과 View

둘 사이의 소통은 불가능하다.

이유1: 모델은 UI와 독립적이지만, View는 UI에 의존한다.(View는 UI와 관련된 것들만을 포함한다)<br>
이유2: View는 일반 객체일 뿐이다. (버튼 그 자체가 무슨일을 하는지는 알수가 없다.)


### View와 Controller

**Controller > View**: View는 일반적으로 Controller의 하인들이다.
- Controller는 `outlet`을 통해 View 객체들을 모두 관리할 수 있다.

**View > Controller**: 구조적으로 미리 정해진 방식을 통해 Controller에게 행위에 대한 요청(delegate)과 데이터에 대한 요청(data source)을 할 수 있다.

> 구조적: 이 통신을 하기로 했을 때 일반적인 객체는 어떻게 컨트롤러와 대화를 할지 조금 먼저 생각해놓고 있어야 한다는 것

더 나아가 `targer-action`의 구조를 통해 사용자의 행위에 따라 필요한 함수를 호출할 수도 있다.
- target: 컨트롤러가 해야하는 건 자신에게 타겟을 만드는 것
- action: UIButton이나 다른 것들은 액션을 가지고 버튼을 누를때마다 타겟을 호출

```
scrollView?

ScrollView가 이미지 같은 걸 스크롤하면 이를 컨트롤러에게 말해줘야 한다.(아래로 더 스크롤할수 있는지, 옆으로 스크롤해도 되는지?와 같은..) 즉, 일을 수행하는 동안 컨트롤러와 대화하길 원한다.
그렇게 하려면 ScrollView가 미리 정의한 메소드를 delegate의 일부로 사용해야 한다.
이 delegate는 ScrollView에 있는 var이고 이 var는 객체를 가지고 있다.
우리가 이 객체에 대해 아는건 이것이 특정 메시지들에 반응을 한다는 것이다.
이는 특정 메시지들에 반응을 한다는 것을 의미한다. 이런 메시지의 대부분에는 will, should, did같은 단어가 들어간다.
```
view는 일반적(객체)이기 때문에 자기가 화면에 표시하고 있는 데이터를 가질 수는 없다.
즉, 화면에 표시하고 있는 데이터를 인스턴스 변수로 가지고 있지 않다는 뜻이다.<br>

더 나아가 아래와 같은 상황을 가정해보자

```
View가 아이팟 음악 라이브러리 전체를 보여주고 있다고 생각해보자
노래는 50,000곡이 있다고 가정한 상황에서 ListView(리스트를 만드는 일반적인 View)가 그 50,000개의 곡을 모두 가져오는 것은 불가능할 것이다. 그 대신 프로토콜을 사용해 다른 방식의 특별한 메시지를 주고받는데 (어느곳에 있는 데이터를 줘라, 항목이 몇개 있나 하는 메시지 등) 그러면 Controller는 이를 구현해 Model에게 이야기하여 데이터를 View에 가져다 준다.

즉, TableView는 iOS에 있는 일반적인 뷰 중 하나인데 아이패드에 음악을 스크롤할 때 이게 아이팟 음악 앱이라고 생각을 하고 노래 리스트를 나열해주는 것이 아닌 단지 데이터를 준다고만 인지한 상태에서 화면에 리스트를 띄우게 된다.

이러한 종류의 delegate를 data source라고 한다.
```

이러한 delegate와 data source는 서로 비슷한데 둘의 차이점은 **다른 메소드들을 가지고 있는 것** 이다.<br>
이때 메소드들은 UI요소들에 의해 좌우된다.

즉, 미리 정해진 리스트가 아니라 UI요소의 상황에 따라 바뀌는 것을 의미한다.


### 복잡한 MVC

<center>
<figure>
<img src="/assets/post-img/swift/11.jpeg" alt="" width="80%">
</figure>
</center>

MVC가 다른 MVC와 소통할 때 다른 MVC를 항상 자신의 뷰로 취급한다.
그래서 세부사항을 모른채로 구조적인 방법으로만 이야기한다. 이는 일반적이고 재사용 가능한 컴포넌트처럼 행동한다.

즉, MVC를 잘 그룹화하는 것이 중요하다!


## Model 만들기

### Concentration.swift

File > New > File > Swift File > 최상위 폴더아래(ViewController.swift와 동일한 위치)에 생성

Model에 class를 생성할 때에는 공개 API가 무엇인지를 항상 생각해야한다.
API란 Application Programming Interface의 약자로 클래스안의 모든 메소드와 인스턴스 변수의 리스트를 의미한다. 공개 API는 다른 클래스들의 사용을 허락한 메소드와 인스턴스 변수들을 의미한다. 결국 이 클래스를 어떻게 사용하는지를 결정하는 것을 뜻한다.

이때 사용자 입장에서 집중력 게임에서 할 수 있는 일은 카드를 뒤집는 일 밖에 할 수 없다. 다른 카드 매칭 같은 것들은 내부에서 구현해야하는 것이다. 사용자의 관점에서는 오직 카드를 `touch`하는 행위만 할 수 있다. 그래서 카드를 고르게 해주는 func이 필요하다.


```swift
import Foundation

class Concentration {
    var cards = [Card]()  // 빈 배열로 초기화
    func chooseCard(at index: Int) {

    }
}
```
카드를 고를때 인덱스를 이용하게 함으로써 다른 종류의 UI에 대해 좀 더 유연하게 대응해준다.


### Card.swift

이제 Card 모델을 만들어보도록 한다. 이 모델또한 UI와는 아무런 관계가 없고 여기서 가장 흥미로운 점은 Card를 구조체로 만드는 것이다.(클래스가 아니다)

#### 구조체와 클래스의 차이

1. 구조체는 상속성이 없다. 이를 통해 구조체를 좀 더 간단하게 만들 수 있다.
2. 구조체는 값 타입이고 클래스는 참조 타입이다.

값 타입은 인자로 보내거나 배열에 넣거나 다른 변수에 할당해도 복사가 가능하다. iOS에서는 배열, 문자열, 정수형, 딕셔너리 모두 구조체이다. 코드안에서 주고받을 때 계속 복사된다. 무언가를 전달할 때 모든 내용을 하나하나 복사하기보다 누군가 내용을 변경했을 때만 실제 복사하도록 하는 전달방식을 취한다.

참조 타입은 힙에 자료형이 담겨있고 그 자료형에 포린터를 쓸 수 있다. 이를 여러군데 사용한다면 실제로 그 자료형을 보내는게 아닌 자료형을 가리키는 포인터를 보내는 것이다. 따라서 코드안에 한 오브젝트를 가리키는 포인터가 잔뜩 있을 수 있다.


다시 Card 모델 생성으로 돌아와서 이 Model안에서는 카드가 무엇을 해야하는지, 어떻게 게임이 진행되어야하는 지에 관한 내용을 작성해준다. Card가 어떻게 보이는지에 대해서는 절대 정의하지 않는다.


```swift
import Foundation

struct Card {
    var isFaceUp = false  // 뒤집어져 있는 것이 기본
    var isMatched = false  // 서로 일치하지 않는 것이 기본
    var identifier : Int  // 카드의 식별자
}
```


### Controller에서 Model을 써보기

#### ViewController.swift

Controller에 Concentration 모델클래스를 연결해보자

```swift
var game : Concentration
```

이제 이 game에겐 어떤 메시지도 전달이 가능해진다. 카드를 가져올수도 있고 카드를 선택할 수도 있다.

그러나 익숙한 에러가 하나 뜰 것이다.

<center>
<figure>
<img src="/assets/post-img/swift/12.jpeg" alt="" width="80%">
</figure>
</center>

초기화가 되지 않았다는 것으로 아래와 같이 코드를 진행해준다.

```swift
var game : Concentration = Concentration()
```

신기하게도 해당 방법을 통해 에러를 해결할 수 있게 될 것이다. Concentration는 클래스이기 때문에 모든 변수들이 초기화되면 인수가 없는 init을 자동으로 가지게 된다. Concentration에서는 변수 cards가 딱 하나있고 cards 변수는 이미 초기화가 되어있었다. 따라서 Concentration은 자동으로 init을 가지게 된다.

그리고 iOS는 강한 타입추론이기에 game이 당연히 Concentration 타입이라는 것을 알 수 있기 때문에 아래와 같이 작성이 가능하다.

```swift
var game = Concentration()
```

### 자체적인 init을 만들어주기

그러나 우리가 가질 카드가 앞으로 몇개인지는 알수가 없다. 따라서 자체적인 init을 만들어줘야 한다.

#### Concentration.swift

```swift

```

구조체가 받는 공짜 이니셜라이저는 모든 변수를 초기화한다. 이미 Card에서 isFaceUp, isMatched과 같이 이미 초기화한 것이 있다고 하더라도 이 변수 모두를 초기화해버린다. 따라서 구조체에서 다시 초기화해줌으로써 Card를 초기화한다.



card가 스스로 자기의 식별자를 지정해주게 한다.
카드가 어떤 숫자를 골라도 상관없이 유일하기만 하면 된다. 이것만 충족시키면 된다. 즉 init이 식별자를 받지 않도록!
유일한 식별자를 주기 위해서 Card.swift에서 static 메서드를 사용한다.

```swift
import Foundation

struct Card {
    var isFaceUp = false
    var isMatched = false
    var identifier : Int

    static func getUniqueIdentifier() -> Int { // 유일한 Int를 리턴한다
      return 0 // 함수가 호출될때마다 유일한 식별자(0)를 리턴할 것이다
    }

    init(identifier: Int) {
        self.identifier = 0
    }
}
```

정적함수는 함수이며 Card클래스 안에 있지만 Card에게 보낼 수 없다. Card는 이 메시지를 전혀 이해하지 못하고 메시지를 이해할 수 있는 것은 Card의 타입뿐이다. 이 함수를 전역함수나 유틸리티 함수 도는 그냥 타입에 붙어있는 함수로 생각하면 된다. Card에게 유일한 식별자를 달라고 요청할 수도 없다. Card 타입 자체에게 요청한다.

그래서 함수를 부르고 싶다면 타입에게 부른다. > `Card.getUniqueIdentifier()`


import Foundation

class Concentration {
    var cards = [Card]()
    func chooseCard(at index: Int) {

    }

    init(numberOfPairsCards: Int) {
        for identifier in 1...numberOfPairsCards {
            let card = Card()
            cards += [card, card]
        }
    }
}


스위프트에서의 _ 는 무시하라는 의미이거나 다시 쓰지 않을 거라 어떤 것이어도 상관없다는 의미



### 카드를 섞기


<center>
<figure>
<img src="/assets/post-img/swift/13.jpeg" alt="" width="80%">
</figure>
</center>

인스턴스 멤버인 cardButtons를 사용할 수 없다. 이 cardButtons은 속성 이니셜라이저 안에서 사용이 불가능하다. game변수는 속성이고 초기화하고 있으며 속성 이니셜라이저는 self가 존재하기 전에 실행되어야 한다.

스위프트에서는 어떤 거라도 사용하기 전에는 완전히 초기화를 해야한다. 어떤 변수에 접근하거나 어떤 함수를 부르거나 여튼 무엇을 하든 초기화가 되어있어야 한다. 그러나 아직 우리는 game을 초기화하는 중이었기 때문에 완벽한 초기화가 되어있지 않다. 이렇게 지금 상황은 하나가 다른 하나에 의존하고 있는 상황이다.

이를 해결하는 방법이 있다.

1. lazy

```swift
lazy var game = Concentration(numberOfPairsCards: (cardButtons.count + 1) / 2)
```

어떤 변수를 lazy로 만들면 누가 사용하기 전까지는 초기화하지 않는다. 누군가 game을 사용하려 할 때 초기화를 한다. 즉 cardButtons 변수가 초기화될때까지 아무도 game변수를 사용할 수 없다. 이렇게 lazy를 사용하면 초기화가 되었다고 쳐주게 된다. 그러나 이런 lazy를 사용할때 한가지 제약사항이 있다. lazy가 되면 didSet을 가질 수 없다.

```swift
lazy var game = Concentration(numberOfPairsCards: (cardButtons.count + 1) / 2) {
  didSet {}
}
```

<center>
<figure>
<img src="/assets/post-img/swift/13.jpeg" alt="" width="80%">
</figure>
</center>

lazy변수는 속성관찰자를 사용할 수 없다는 의미이다. 속성 관찰자를 사용하려면 game이 변하는 모든 곳을 찾아서 다른 방법으로 해야한다.



```swift
func updateVireFromModel() {
    for index in cardButtons.indices {

    }
}
```

indices는 배열의 메소드로 모든 인덱스의 계수 가능 범위를 배열로 리턴해준다.


arc4:유사(pseudo) 임의 번호 생성기이다. 0부터 상한 사이의 숫자를 임의로 생성해준다.

---
layout: post
title: swift 기본문법 - 중첩타입(Nested types)
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
[참고](https://xho95.github.io/swift/language/grammar/nested/2017/03/02/Nested-Types.html)

<hr>

## 중첩타입(Nested types)

열거형(Enum)은 종종 특정 클래스나 구조체 기능을 지원하기 위해 만들어진다.

마찬가지로, 보다 복잡한 타입의 컨텍스트내에서 사용하기 위해, 유틸리티 클래스 및 구조체를 정의하는 것이 편리할 수 있습니다.<br>
이를 달성하기 위해 Swift는 중첩된 타입(Nested Types)을 정의할 수 있는데 즉, 지원하는 타입의 정의내에서 클래스 및 구조체, 열거형을 중첩할 수 있다.

다른 타입에서 타입을 중첩하기 위해서는 타입의 괄호 밖에 정의해야 하며, 타입은 필요한 만큼의 여러 수준으로 중첩할 수 있다.


### 중첩 타입 사용

BlackjackCard라는 구조체는 블랙잭 게임에서 사용하는 게임 카드로 만들어 정의한다. BalckjackCard 구조체는 Suit와 Rank라는 두가지 열거형 타입을 포함한다.

블랙잭에서는 Ace카드 값이 1또는 11로 이러한 특징은 Values라는 구조체로 표현되며, Rank 열거형 안에 중첩되어 있다.

```swift
struct BlackjackCard {  // 구조체 생성

    // nested Suit enumeration
    enum Suit: Character {  // 구조체 내 열거형 생성, case들의 raw value가 Character이기에 ray type설정필수!
        case Spades = "♠", Hearts = "♡", Diamonds = "♢", Clubs = "♣"
    }

    // nested Rank enumeration
    enum Rank: Int {  // 열거형 하나 더 생성
        case Two = 2, Three, Four, Five, Six, Seven, Eight, Nine, Ten
        case Jack, Queen, King, Ace
        struct Values {  // 열거형 안에 구조체 생성(struct ⊂ enum, enum ⊂ struct 이런형식임)
          // Ace는 first, second 두개의 값을 가지고 이외의 카드는 한가지 값만 가진다.
            let first: Int, second: Int?
        }

        // 열거형에서는 저장 프로퍼티는 만들 수 없지만, 연산 프로퍼티는 만들 수 있다는 점 잊지말자
        var values: Values {  // 연산프로퍼티, values라는 방금 만든 Values 구조체 타입의 프로퍼티 정의
            switch self {  // 여기서의 self는 Rank를 의미한다
            case .Ace:  // 각 케이스마다 Values 타입의 인스턴스를 리턴
                return Values(first: 1, second: 11)
            case .Jack, .Queen, .King:
                return Values(first: 10, second: nil)
            default:
            // Rank의 첫번째 case Two부터 2라는 값을 주고 있고, 따라서 나머지 케이스에는 1씩 증가한 값이 할당된다.
            // Three의 rawValue는 3이 된다 >> 리턴하는 Values의 인스턴스가 values에 할당되지 않는다(연산 프로퍼티 특징)
                return Values(first: self.rawValue, second: nil)
                }
        }
    }

    // BlackjackCard properties and methods
    // rank와 suit는 BlackjackCard 구조체의 저장 프로퍼티이다(위에 선언한 Rank, Suit 열거형 타입의)
    let rank: Rank, suit: Suit

    // description또한 BlackjackCard의 연산 프로퍼티
    var description: String {
      // 연산 프로퍼티 내 output 선언(output은 description안에서만 쓸수 있는 지역변수)
        var output = "suit is \(suit.rawValue),"  // suit.rawValue는 Suit열거형에서 나열한 기호들
            output += " value is \(rank.values.first)"  // values의 first값
            if let second = rank.values.second {  // 해당 카드가 ace카드였다면 if let(옵셔널 바인딩)으로 values에 second값 있는지 확인
                output += " or \(second)"  // second값이 있다면 넣어준다
            }
            return output
    }
}
```

이렇게 비로소 BlackjackCard 구조체를 정리해볼 수 있다. 그럼 진짜 BlackjackCard 구조체의 인스턴스를 생성해보자.

```swift
// BlackjackCard 타입의 theAceOfSpades라는 인스턴스 생성
let theAceOfSpades = BlackjackCard(rank: .ace, suit: .spades)  // rank, suit는 BlackjackCard의 저장프로퍼티
print("theAceOfSpades: \(theAceOfSpades.description)")  // output을 리턴해주는 description 연산프로퍼티

// theAceOfSpades: suit is ♠, value is 1 or 11
```

**theAceOfSpades: suit is ♠, value is 1 or 11**

Rank와 Suit가 BlackjackCard에 중첩되어 있지만, 타입을 컨텍스트에서 유추할 수 있기에 인스턴스를 초기화하면 case 이름만으로 열거 case를 참조할 수 있게 된다.



### Referring to Nested Types

정의 컨텍스트 외부에서 중첩된 타입을 사용하려면, 해당 이름앞에 nested type의 이름을 붙이면 된다.

```swift
let heartsSymbol = BlackjackCard.Suit.hearts.rawValue
// heartsSymbol is "♡"
```

따라서 BlackjackCard라는 인스턴스를 만들지 않고 해당값에 접근하고 싶다면, 위 코드처럼 nested type이름을 붙이면 된다.

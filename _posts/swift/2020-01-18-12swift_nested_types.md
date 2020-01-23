---
layout: post
title: swift 기본문법 - 중첩타입(Nested types)
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 중첩타입(Nested types)

열거형(Enum)은 종종 특정 클래스나 구조체 기능을 지원하기 위해 만들어진다.

마찬가지로, 보다 복잡한 타입의 컨텍스트내에서 사용하기 위해, 유틸리티 클래스 및 구조체를 정의하는 것이 편리할 수 있습니다.<br>
이를 달성하기 위해 Swift는 중첩된 타입(Nested Types)을 정의할 수 있는데 즉, 지원하는 타입의 정의내에서 클래스 및 구조체, 열거형을 중첩할 수 있다.

다른 타입에서 타입을 중첩하기 위해서는 타입의 괄호 밖에 정의해야 하며, 타입은 필요한 만큼의 여러 수준으로 중첩할 수 있다.


### 중첩 타입 사용

BalckjackCard라는 구조체는 블랙잭 게임에서 사용하는 게임 카드로 만들어 정의한다. BalckjackCard 구조체는 Suit와 Rank라는 두가지 열거형 타입을 포함한다.

블랙잭은 일에서 십일까지 값인 에이스카드를 가진다. 이러한 특징은 Values라는 구조체를 표현하는데 Rank 열거형 안에 중첩되어 있다.

```swift
struct BlackjackCard {

    // nested Suit enumeration
    enum Suit: Character {
        case Spades = "♠", Hearts = "♡", Diamonds = "♢", Clubs = "♣"
    }

    // nested Rank enumeration
    enum Rank: Int {
        case Two = 2, Three, Four, Five, Six, Seven, Eight, Nine, Ten
        case Jack, Queen, King, Ace
        struct Values {
            let first: Int, second: Int?
        }
        var values: Values {
            switch self {
            case .Ace:
                return Values(first: 1, second: 11)
            case .Jack, .Queen, .King:
                return Values(first: 10, second: nil)
            default:
                return Values(first: self.toRaw(), second: nil)
                }
        }
    }

    // BlackjackCard properties and methods
    let rank: Rank, suit: Suit
    var description: String {
        var output = "suit is \(suit.toRaw()),"
            output += " value is \(rank.values.first)"
            if let second = rank.values.second {
                output += " or \(second)"
            }
            return output
    }
}
```

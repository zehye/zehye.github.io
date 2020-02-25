---
layout: post
title: 인코딩과 디코딩, Codable 프로토콜
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 인코딩과 디코딩

- 인코딩(Encoding):정보의 형태나 형식을 표준화, 보안, 처리 속도 향상, 저장 공간 절약 등을 위해서 다른 형태나 형식으로 변환하는 처리 혹은 그 처리 방식(출처 : [위키백과 - 부호화](https://ko.wikipedia.org/wiki/%EB%B6%80%ED%98%B8%ED%99%94))
- 디코딩(Decoding): 인코딩의 반대 작업을 수행하는 것

또, 인코더(Encoder)는 부호화를 수행하는 장치나 회로, 컴퓨터 소프트웨어, 알고리즘을 뜻함!


### Codable

스위프트4 버전에서는 스위프트의 인스턴스를 다른 데이터 형태로 변환하고 그 반대의 역할을 수행하는 방법을 제공한다. 스위프트의 인스턴스를 다른 데이터 형태로 변환할 수 있는 기능을 **Encodable 프로토콜로 표현** 하였고, 그 반대의 역할을 할 수 있는 기능을 **Decodable로 표현** 해 두었으며, **둘을 합한 타입을 Codable로 정의** 해두었다.

```swift
typealias Codable = Decodable & Encodable
```

Codable은 스위프트 4 버전에서 처음 소개한 프로토콜로 Codable은 다양한 상황에서 사용할 수 있다. 예를 들어 JSON 형식으로 서버와 애플리케이션이 통신한다면 Codable 프로토콜을 이용해 편리하게 인코딩 및 디코딩할 수 있다.


<center>
<figure>
<img src="/assets/post-img/iOS/63.png" alt="" width="80%">
</figure>
</center>


- Decodable: 스위프트 타입의 인스턴스로 디코딩할 수 있는 프로토콜
- Encodable: 스위프트 타입의 인스턴스를 인코딩할 수 있는 프로토콜


### 선언 예제

#### Codable

Coordinate 타입과 Landmark 타입의 인스턴스를 다른 데이터 형식으로 변환하고 싶은 경우에 Codable 프로토콜을 준수하도록 하면된다. Codable 타입의 프로퍼티는 모두 Codable 프로토콜을 준수하는 타입이어야 하며, 스위프트의 기본 타입은 대부분 Codable 프로토콜을 준수한다.

```swift
struct Coordinate: Codable {
	var latitude: Double
	var longitude: Double
}

struct Landmark: Codable {
    var name: String
    var foundingYear: Int
    var vantagePoints: [Coordinate]
    var metadata: [String: String]
    var website: URL?
}
```

#### CodingKey

자주 사용하게 될 **JSON** 형태의 데이터로 상호 변환하고자 할 때는 기본적으로 인코딩/디코딩할 JSON 타입의 키와 애플리케이션의 사용자정의 프로퍼티가 일치해야 한다.

만약 JSON의 키 이름을 구조체 프로퍼티의 이름과 다르게 표현하려면 타입 내부에 String 타입의 원시값을 갖는 CodingKeys라는 이름의 열거형을 선언하고 CodingKey 프로토콜을 준수하도록 하면 된다. CodingKeys 열거형 케이스의 이름은 해당 프로퍼티의 이름과 일치해야 하며 프로퍼티의 열거형 케이스의 값으로 매칭할 JSON 타입의 키를 할당하면 된다. 만약, JSON 타입의 키와 프로퍼티 이름이 일치한다면 값을 할당하지 않아도 무방하다!

```swift
struct Landmark: Codable {
    var name: String
    var foundingYear: Int
    var location: Coordinate
    var vantagePoints: [Coordinate]

    enum CodingKeys: String, CodingKey {
        case name = "title"
        case foundingYear = "founding_date"

        case location
        case vantagePoints
    }
}
```

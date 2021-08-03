---
layout: post
title: required init?(coder:)란 무엇인가?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## 문제 상황

UIView와 UIViewController를 상속해 지정이니셜라이저(init)을 작성하면 아래와 같은 에러를 마주하게 된다.

```
'required' initializer 'init(coder:)' must be provided by subclass of 'UIViewController'
```

이 말은 즉슨, 'required' 이니셜라이저인 `init(coder)`를 정의해주어야 한다는 의미로 아래와 같이 작성해주면 에러는 사라진다.

```swift
required init(coder: NSCoder) {
  fatalError("init(coder) has not been implemented")
}
```

이건 뭘까? >> 바로 NSCoding 떄문에 발생하는 에러이다.

```swift
public protocol NSCoding {
  public func encode(with aCoder: NSCoder)
  public init?(coder aDecoder: NSCoder)
}
```

NSCoding 프로토콜에 대해서는 다음에 좀더 깊이 정리해볼 것이다.

간단하게 말하자면 NSCoding 프로토콜은 이를 구현하는 클래스로부터 실패가능한 이니셜라이저를 작성하도록 한다.<br>
프로토콜에 적혀있는 이니셜라이저를 구현하면 `required`키워드가 붙으며 이 `required`키워드가 붙은 이니셜라이저를 상속받는 자식 클래스에서도 이를 반드시 구현해줘야 한다.

UIView와 UIViewController는 NSCoding 프로토콜을 구현하고 있기 때문에, 이를 상속받은 클래스에서는 반드시 `required init?(coder: )`를 구현해줘야 한다.

swift에서는 자식 클래스에서 지정이니셜라이저를 따로 작성하지 않은 경우, 부모의 이니셜라이져들을 자동으로 상속한다.<br>
그래서 새로운 지정 이니셜라이저를 자식 클래스에서 작성하지 않았을 경우에는 위같은 에러가 발생하지 않았던 것이다.

즉, 자식클래스에서 새로운 지정 이니셜라이저를 작성하게 된다면, 부모 클래스의 이니셜라이저들이 자동 상속되지 않기때문에<br>
`required init?(coder: )`를 구현하라는 에러가 발생하는 것이다.  

---
layout: post
title: masksToBounds와 clipsToBounds 차이점?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## 예시 상황

어떤 네모난 textView 안에 "안녕하세요"라는 텍스트가 들어있다고 생각해보자.<br>
이때 이 네모난 textView를 동그랗게 즉, radius를 줬다고 하자. 그러면 textView의 텍스트는 어떻게 될까?

아마도 네모난 부분에서 동그랗게 변해진 부분에 존재하는 텍스트는 동그래진 만큼 잘려서 보이지 않게 될 것이다.

때에 따라서는 이 텍스트가 잘리면 안되는 상황도 분명 존재할텐데! <br>
그때 바로 사용하는 것이 clipsToBounds, masksToBounds이다.

우선 이 둘이 하는 기능 자체는 똑같은데, 불러오는 곳이 다르다.


```swift
textView.layer.masksToBounds
textView.clipsToBounds
```

즉, masksToBounds는 layer의 프로퍼티이고 clipsToBounds는 그냥 textView의 프로퍼티이다.<br>
이때 알아두면 좋을 건 textView 뿐만 아니라, 버튼, 라벨에서도 불러올 수 있는 프로퍼티라는 것!


### masksToBounds

```swift
open var masksToBounds: Bool
```

masksToBounds의 기본값은 false이다. 그렇다면 이때 true, false가 의미하는것은 무엇일까?

- true: textView의 테두리가 기준으로, 위에 설명처럼 텍스트는 잘리게 된다.
  - textView라는 큰 뷰가 있고 그 안에 "안녕하세요"라는 텍스트가 있는것으로 큰 뷰를 기준으로 바깥에 있는것은 잘리게 된다.
- false: textView 안의 내용 또한 잘리지 않고 "안녕하세요" 모두 보이게 된다.


### clipsToBounds

```swift
open var clipsToBounds: Bool  // When Yes, content and subviews are clipped to the bounds of the view. Default is No
```

clipsToBounds 또한 masksToBounds와 같이 Bool 타입이다.<br>
주석 내용또한 살펴보면 true일때 내용들과 서브뷰들은 뷰의 테두리를 기준으로 잘리게 되며 default는 false이다.

masksToBounds와 같이 false를 주지 않으면 안의 내용은 잘리게 된다.

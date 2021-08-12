---
layout: post
title: iOS Configuring the Event-Related Behavior > isUserInteractionEnabled, isMultipleTouchEnabled, isExclusiveTouch
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Configuring the Event-Related Behavior

기존 코드에 isUserInteractionEnabled가 사용되는것을 보곤 이게 뭐지? 하고<br>
UIView의 도큐먼트를 살펴보게 되었습니당. Configuring the Event-Related Behavior라고 하면서<br>
3개의 프로퍼티가 있는것을 보게 되었다..



### isUserInteractionEnabled

유저의 이벤트가 event queue로부터 무시되고 삭제됐는지를 판단하는 boolean 값

만약 `isUserInteractionEnabled == false`를 하면 view를 위한 touch, press, keyboard 그리고 focus event는 event queue에서 무시되고 삭제된다. 반대로 `isUserInteractionEnabled == true`를 하게 된다면 event는 정상적으로 뷰에 전달이 된다. default는 true이다.

그런데 이를 사용할때에도 예외는 있나보다.

애니메이션을 적용하는 동안에는 이 프로퍼티의 값이 false든 true든 상관없이 user interaction은 disable된다고 한다.<br>
애니메이션이 일어나는 뷰를 포함해서 말이다. 이를 막고싶다면 애니메이션 옵션으로 `allowUserInteraction`을 주면 된다고 한다.



### isMultipleTouchEnabled

뷰가 한번에 2개 이상의 터치를 수신하는지 여부를 나타내는 boolean 값

만약 `isMultipleTouchEnabled == true`를 설정하면 multi-touch sequence 와 관련된 모든 터치를 받고 뷰의 bounds에서 시작한다. 반면 `isMultipleTouchEnabled == false`를 설정하면 오직 multi-touch sequence 안에 있는 first touch event만 받는다고 한다. default는 false라고 한다.

알아둬야 할 점은 이 프로퍼티는 gesture recognizer에 영향을 끼치지 않는다는 점이다. <br>
gesture recognizer는 view에서 일어나는 모든 터치를 수신한다.

그리고 이 프로퍼티가 false로 설정되어있다고 할때, 같은 윈도우 안에있는 다른 뷰들은 여전히 터치 이벤트를 받을 수 있다는 특징이 잇다. 이런때에 multi-touch event를 서로 배타적으로 다루고 싶다면, `isExclusiveTouch`프로퍼티와 `isMultipleTouchEnabled` 프로퍼티를 둘다 true로 설정하면 된다고 한다.




### isExclusiveTouch

리시버가 터치이벤트를 exclusively하게 다룰지를 나타내는 boolean 값

만약 `isExclusiveTouch == true`로 설정하면 동일한 윈도우의 다른 뷰에 대한 터치이벤트를 차단한다. 반면 `isExclusiveTouch == false`를 한다면 차단하지 않는다. default는 false 값이다.

동일한 윈도우안에서 어떤 뷰가 터치이벤트를 전달하면 다른 뷰에 대한 터치이벤트는 차단시키는 것이다.

동일한 윈도우에서 서로 다른 뷰의 터치 이벤트 등을 처리할때 서로 관계없이 작업이 진행되게 하기 위해서 단순히 `isExclusiveTouch == true`를 하면 되는것같아 보이지만 문서에서는 이 뿐만 아니라 `isMultipleTouchEnabled == true`까지 진행해줘야 한다고 적혀있다. 

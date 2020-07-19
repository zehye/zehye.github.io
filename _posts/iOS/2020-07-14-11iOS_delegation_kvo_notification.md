---
layout: post
title: Delegation, Notification, KVO
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Delegation, Notification center, KVO

Delegation, Notification center, KVO는 iOS를 만들다보면 자주 나오는 패턴들이다. KVO를 제외하면 실제 매우 자주 사용해본 경험도 있을 것인데, 대부분의 경우 View와 VC 간 또는 각각의 것들 사이에서 소통이 필요할 때 사용했을 것이다. 어떤 이벤트가 A에서 발생하면 B에 알려주어 적절한 조치를 취하도록 하는 것.


그렇다면 이 세가지 패턴은 왜 만들어지게 되었을까?

> 하나의 객체가 다른 객체와 소통은 하되 묶이기(coupled)는 싫어서..

세 패턴 모두 특정 이벤트가 일어나면 원하는 객체에 알려주어 해당되는 처리를 하는 방법을 가지고 있다. 어플리케이션의 특성 상 객체 간 소통은 필수적이다. 하지만 한 객체는 그 자체로 존재하면서 소통하고 싶을 뿐 다른 객체에 종속되어 동작하는 것은 **재사용성** 과 **독립된 기능 요소** 측면에서 바람직하지 않다. 대표적으로 VC는 한 View를 관리하는 독립적인 객체로 존재해야지 소통을 위해 다른 VC에 묶여 동작하는 것은 올지 않은 방식이다. 따라서 이때 Delegation, Notification, KVO를 사용한다.



### Delegation

Delegate은 보통 프로토콜을 정의하여 사용한다.



### Notification

Notification center라는 싱글턴 객체를 통해 

### KVO

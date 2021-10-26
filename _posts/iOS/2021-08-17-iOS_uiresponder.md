---
layout: post
title: iOS UIResponder 란?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## UIResponder

리스폰더 객체, 즉 UIResponder의 인스턴스는 UIKit 앱 이벤트 처리 백본을 구성한다.<br>
많은 핵심 객체는 UIApplication객체 및 모든 UIView객체(UIWindow포함)를 포함하여 리스폰더이다.

즉, UIApplication, UIViewController, UIView 객체 모두 리스폰더라는 이야기이다.

**이벤트가 발생하면, UIKit는 이를 처리할 수 있도록 앱의 리스폰더 객체에 전달한다.** <br>
이는 또 다시말하면, 이벤트가 발생하면 UIApplication, UIViewController, UIView객체가 해당 이벤트를 다룰 수 있다는 의미다.


이벤트에는 터치 이벤트, 모션이벤트, 원격제어 이벤트 및 press이벤트를 비롯한 여러 종류의 이벤트가 존재한다. 특정 타입의 이벤트를 처리하려면, 리소폰더가 해당 메소드를 오버라이드 해야한다.(To handle a specific type of event, a responder must override the corresponding methods) 예를들어 터치이벤트를 처리하기 위해 리스폰더는 `touchesBegan(_:with), touchesMoved(_:with), touches Ended(_:with), touchedCancelled(_:with)`를 구현해야한다. 이렇게 터치이벤트의 경우 리스폰더는 UIKit에서 제공한 이벤트 정보를 사용해 해당 터치의 변경사항을 추적하고, 앱의 인터페이스를 적절하게 업데이트 해야한다.

이벤트 처리 외에도 UIKit 리스폰더는 **처리되지 않은 앱의 다른 부분을 전달하는 작업을 관리한다.**

주어진 리스폰더가 이벤트를 처리하지 않으면, 리스폰더 체인의 다음 이벤트로 해당 이벤트를 전달한다. UIKit는 미리 정의된 규칙을 사용해 리스폰더 체인을 동적으로 관리하여 어떤 객체가 이벤트를 수신한 다음에 어떤 객체를 선택할지 결정한다. 예를들어, 뷰는 이벤트를 상위 뷰로 전달하고, 계층의 루트 뷰는 이벤트를 해당 뷰컨으로 전달한다.

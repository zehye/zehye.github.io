---
layout: post
title: becomeFirstResponder와 resignFirstResponder란?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


회사에서 코드를 보는데 resignFirstResponder 함수를 사용하는것을 보고 도대체 이게 뭘까? 하고 궁금증이 생겼다..<br>
키보드를 올리고 내릴때, 특히 내릴때 이 함수를 계속해서 호출하는데 이럴때 사용하는 방법 중 하나라고만 하고 정확한 정의를 모르곘어서 한번 찾아서 정리해야겠다고 생각했고... 그래서 정리해보려고 한다..!!


## becomeFirstResponder

해당 윈도우에서 객체를 first responder로 만들것을 요청하는 함수<br>
해당 객체가 첫번째 응답 responder이면 true, 아니면 false를 리턴한다.

여기서 responder는 이벤트에 대한 응답을 처리하기 위한 추상 인터페이스를 뜻한다.<br>
UIView, UIViewController, UIApplication 객체들이 responder에 해당한다고 생각하면 된다. <br>
이벤트가 발생하면 UIKit는 이를 처리하기 위해 우리 앱의 responder에게 이벤트를 보낸다.

현재의 객체를 first responder로 지정하려면 이 메소드를 호출하면 된다.<br>
즉 UITextView나 UITextField에서 키보드를 올리는 액션을 취하기 위해서는 이 메소드를 호출하면 된다.

하지만 호출한다고 해서 무조건 first responder가 되는건 또 아니라고 한다. UIKit에게 현재 first responder에게 resign을 요청하지만 원하는 대로 되지 않는 경우도 있기 때문이다. 이 케이스는 UIKit 객체의 `canBecomeFirstresponder()`를 호출하는데, 이 참수는 기본적으로 false를 반환하기 때문에 객체가 first responder가 되는데 성공하기 위해서는 이 이후 이벤트가 먼저 이 객체로 전달되고 UIKit는 객체의 input view를 보여주려고 시도하게 된다.



## resignFirstResponder

해당 객체에게 지금 윈도우의 첫번째 responder로서의 상태를 포기하라고 요청이 왔음을 알리는 함수<br>
첫번째 responder 상태를 물려주면서 true를 리턴한다.

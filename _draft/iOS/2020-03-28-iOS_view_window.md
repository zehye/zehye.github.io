---
layout: post
title: view와 window는 무엇일까?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/27.png" alt="" width="50%">
</figure>
</center>

view = 그림 <br>
window = 액자

위 이미지만 보고 나는 당연하게도 UIWindow가 UIView의 부모구나! 라고 생각했는데, 그게 아니라고 한다..............뚜둔<br>
오히려 UIView가 UIWindow의 부모라고................!?


## View

뷰는 UIView 클래스의 인스턴스로 윈도우 위에서 컨턴츠를 보여준다. 즉 화면에 나타나는 거의 모든 요소를 뷰의 컨텐츠라고 생각하면 쉽다. 뷰는 컨텐츠를 나타내고 터치, 하위 뷰의 배치등의 역할을 수행한다. 뷰는 원하는 UI를 만들기위해 쌓아올려야하는 블럭으로 보아도 무방하다. 하나의 뷰로 모든 정보를 담기보다는 여러개의 뷰를 계층구조로 쌓아올려 UI를 표현하게 된다.


## Window

UIWindow는 화면에 나타나는 view를 묶고, UI 배경을 제공하고, 이벤트 처리행동을 제공하는 객체이다. 화면에 표시되는 view들은 모두 window로 묶여있다. 보통은 하나의 윈도우만 사용하는 데, 필요에 따라 개발자가 여러개의 윈도우를 만들어 뷰를 추가할 수도 있다. 뷰와 윈도우 모두 코드를 통해 작성할 수도 있지만 인터페이스 빌더 긴으을 사용해 이를 작성하는 방법 또한 간단하다.
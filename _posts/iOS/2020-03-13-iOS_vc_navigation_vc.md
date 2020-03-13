---
layout: post
title: UINavigationController와 UIViewController 차이점
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## NavigationController

<center>
<figure>
<img src="/assets/post-img/iOS/100.png" alt="" width="80%">
</figure>
</center>

- 계층구조로 구성된 content를 순차적으로 보여주는 container view controller
- stack 구조로 구현 > navigation stack
- 계층 구조 탐색으로 앱 content를 보여주기 적절
- 한번에 한 child view controller의 content만 보여짐

이는 트리 구조처럼 상위 카테고리에서 점차 하위 카테고리로 넓어져 가는 구조를 표현하며 상위 카테고리로 돌아가기 위해서는 가장 최근에 보여진 뷰컨트롤부터 역순으로 거쳐 가야한다. 따라서 스택구조가 이를 구현하기에 적절하다. 이러한 네비게이션 뷰 컨트롤러는 pop/push메소드를 사용하여 보여지는 child view controller를 변경이 가능하다.


### Navigation Bar > UINavigationBar

<center>
<figure>
<img src="/assets/post-img/iOS/99.png" alt="" width="80%">
</figure>
</center>

- 화면 상단에 있는 항상 보여지는 바
- root view 외의 모든 view 에서 back버튼이 있어 user가 계층구조에서 다시 뒤로 올라갈 수 있게끔 함
- 현재 stack의 top level에 있는 view controller가 변하면 그에 맞게 navigation controller의 navigation bar도 변함
- navigation bar button item: left, middle, right
- View controller - title property 설정시 navigation bar의 가운데에 표시됨

>  Stack에 다음 View Controller를 쌓으며 디스플레이하는 것이 Push, 이전의 View Controller로 되돌아가는 것이 Pop 액션이다. Pop 액션에는 최초에 디스플레이됐던 View Controller로 돌아가는 Pop to Root 액션이 포함되어 있다.


### Optional Tool Bar

- Tool bar는 현재 위치한 content에서 할 수 있는 조작을 보여주는 bar
- tab bar는 content 간 전환에 사용됨
  - tool bar 는 현재 content 내에서의 가능한 동작을 보여주는 용도

- Tool Bar 보여주기
  - isToolBarHidden property == false 이면 tool bar displayed
  - Interface Builder 에서 ✅ Show Toolbar
- Tool Bar item 추가하기
  - toolbar가 보여질 때, 현재 활성화된(active) view controller의 toolBarItems property를 보여줌
  - view controller 의 toolBarItems (UIBarButtonItem type) 에 객체 넣어주면 tool bar에 item이 보임


## View Controller

<center>
<figure>
<img src="/assets/post-img/iOS/101.png" alt="" width="80%">
</figure>
</center>

- 뷰를 제어하는 컨트롤러 객체
- view 프로퍼티를 가짐으로써 프로퍼티로 가지고 있는 뷰와 서브뷰의 레이아웃이나 모양, 컨텐츠를 변경
- 뷰 내의 컨트롤을 사용자가 조작할때 호출되는 액션을 처리
- 뷰의 라이프 사이클을 관리
- 모든 뷰가 뷰컨트롤러를 가질 필요는 없으며 앱의 전체화면 영역을 차지하는 뷰마다 1개씩 있으면 됨

> 뷰 컨트롤러는 단일 루트 뷰를 관리하며 자체에는 여러 개의 하위 뷰가 포함될 수 있다. 해당 뷰 계층과의 사용자 상호 작용은 필요에 따라 앱의 다른 객체와 조정되는 뷰 컨트롤러에서 처리한다.

- 모든 앱에는 컨텐츠가 기본 창을 채우는 하나 이상의 뷰 컨트롤러가 있다
- 앱에 한 번에 화면에 표시 할 수있는 것보다 더 많은 콘텐츠가있는 경우 여러 뷰 컨트롤러를 사용하여 해당 콘텐츠의 다른 부분을 관리


### Two types of view Controllers

- Content View Controllers
  - 모든 뷰를 단독으로 관리
  - UIViewController, UITableViewController, UICollectionViewController등
- Container View Controllers
  - 자체뷰 + 하나 이상의 자식 뷰 컨트롤러가 가진 루트뷰 관리
  - 컨터에너는 컨텐츠를 관리하는 것이 아니라 루트뷰만 관리하며 컨테이너 디자인에 따라 크기 조정
  - UINavigationController, UITabbarController, UIPageViewController 등


> ViewController는 Responder 객체다. 직접 이벤트를 받아 처리하는것이 가능하나 일반적으로 지양!   
뷰가 그 자신의 터치 이벤트를 연관된 객체(보통 뷰컨트롤러)에 action 메서드나 delegate로 전달

#### The Root View Controller

- UIWindow는 그 자체로는 유저에게 보여지는 콘텐츠를 가지지 못한다
- Window는 정확히 하나의 RootViewController를 가지고 이를통해 컨텐츠를 표현

<center>
<figure>
<img src="/assets/post-img/iOS/104.png" alt="" width="80%">
</figure>
</center>

#### Container View Controller

<center>
<figure>
<img src="/assets/post-img/iOS/102.png" alt="" width="80%">
</figure>
</center>

#### Presented View Controller

<center>
<figure>
<img src="/assets/post-img/iOS/103.png" alt="" width="80%">
</figure>
</center>



<hr>

궁극적으로 네이게이션 컨트롤러는 스택같은 계층구조임을 떠올리면 된다. 예로 들어 생각해보면 아래와 같다.

> 설정 -> 내이름 터치 > 이름, 전화번호, 이메일 > 이름

이런식으로 하나의 갈래에서 정보를 파고파고 들어가는 형태이다. 이때 각 뷰컨트롤러의 데이터는 우리가 push해서 들어갈때마다 이전 정보를 저장해주고 있으며 그렇기 때문에 우리가 현재 화면에서 이전화면으로 돌아간다하더라도(pop) pop을 통해 타고타고 돌아가는것이 가능하다. 맨 첫화면으로 바로 돌아갈 수도 있는데 이를 잘 활용하기 위해서는 반드시 root view controller를 지정해주는 것이 필요하다!

반면 뷰 컨트롤러는 각 뷰컨트롤러마다의 데이터를 다른 뷰 컨트롤러가 저장을 해놓고 있지 않다. 우리가 만약에 첫번째 뷰 컨트롤러에서 두번째 뷰 컨트롤러, 세번째 뷰 컨트롤러로 타고타고 넘어갔다고 하자. 현재 유저의 화면이 세번째 뷰 컨트롤러에 있다면 그 뷰컨트롤러에서는 첫번째 뷰 컨트롤러의 데이터를 가지고 있지 않다. 따라서 세번째에서 첫번쨰로 넘어가려고 한다면 그 방법이 존재하지 않는다. 그 이유는 첫번째 뷰 컨트롤러에 대한 정보를 가지고 있지 않기 때문이다.

뷰 컨트롤러의 present는 단순히 덮어씌우기 기능이라고 생각하면 쉽다. 첫번째 뷰 컨트롤러의 정보를 두번째 뷰 컨트롤러에 덮어씌운다. 그렇기 때문에 바로 전 뷰 컨트롤러의 정보에 대해서만 간직하고 있다. 우리가 dismiss를 두번한다고 해서 세번째에서 첫번째 뷰 컨트롤러로 접근할수가 없다.

개발을 할때 정보의 흐름이 하나로 주욱 이어지는 화면을 구현해야한다면 네비게이션 컨트롤러를 사용하고, 각각의 독립적인 뷰를 보여줘야한다면 뷰 컨트롤러를 사용하는게 옳다고 생각한다. 그렇지만 이 둘을 또 각각 다른 단독적인 애들이라고 생각하기 보다는 기본적으로 우리는 뷰 컨트롤러를 사용하고 그 안에서 정보의 흐름이 이어지는 구성이 필요하다면 네비게이션 컨트롤러를 선택적으로 사용한다 라고 생각하는게 옳은 것 같다. 

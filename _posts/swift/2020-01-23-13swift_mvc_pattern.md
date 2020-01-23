---
layout: post
title: MVC(Model-View-Controller Design Pattern)
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## MVC 디자인 패러다임

일단 기본적으로 우리 시스템 안의 모든 객체는 세가지로 나뉜다. > **Model, View, Controller**

<center>
<figure>
<img src="/assets/post-img/swift/45.png" alt="" width="80%">
</figure>
</center>


#### 1. Model

앱에서 '무엇'에 해당하는 UI와 독립적인 객체들이다. 집중력게임에서는 집중력게임을 할 줄 아는 부분에 해당하는 앱이다.
카드가 매치되는지 확인하고, 가져가며 언제 카드를 뒤집어야 하는지 그런 것들을 아는 부분이다. 즉, 집중력 게임에 대한 지식들을 의미한다.

하지만 그게 어떻게 화면에 나오지는 지에 대한 부분은 없다.


#### 2. Controller

'어떻게'에 해당한다. 어떻게 집중력 게임이 화면에 나오는지에 관심을 갖는다.<br>
Model의 정보를 해석하고 구성해서 View에게 보내주는 역할 혹은 사용자와의 상호작용을 모델에게 해석해주는 역할


#### 3. View

컨트롤러의 하인들로 보통 아주 일반적인 UI 요소들인데(UIButton, UILabel, UIViewController) 컨트롤러가 모델과 통신해 앱에서 어떤 것을 UI에 가져오도록 할 때 필요하다.


이러한 MVC는 세가지 캠프 사이의 커뮤니케이션을 관리하는 데에 의미가 있다. 캠프에 객체를 넣으면 서로 얘기할 때 특정한 규칙을 따라야 한다.


## MVC 서로의 관계

<center>
<figure>
<img src="/assets/post-img/swift/10.png" alt="" width="80%">
</figure>
</center>


### Model과 Controller

**Controller > Model**: Controller가 원하는 대로 Model과 이야기 할 수 있다.
- 무엇이 어떤건지에 대해 사용자에게 보여줘야 하기 때문에 모델에 대한 접근이 가능해야 한다.
- 모델의 공개된 모든 기능과는 거의 무제한적인 대화가 가능하다.

**Model > Controller**: 직접적인 소통은 불가능하다.
- Model은 UI와 독립적이고 Controller는 근본적으로 UI에 종속된다.

그러나 소통은 가능하다. Model의 데이터가 변경되었을때 해당 변경사항과 연결된 UI에도 업데이트를 하는 경우!

`Notification & KVO(key value observing)`: Model의 변경사항을 방송하는 것

### Model과 View

둘 사이의 소통은 불가능하다.

이유1: 모델은 UI와 독립적이지만, View는 UI에 의존한다.(View는 UI와 관련된 것들만을 포함한다)<br>
이유2: View는 일반 객체일 뿐이다. (버튼 그 자체가 무슨일을 하는지는 알수가 없다.)


### View와 Controller

**Controller > View**: View는 일반적으로 Controller의 하인들이다.
- Controller는 `outlet`을 통해 View 객체들을 모두 관리할 수 있다.

**View > Controller**: 구조적으로 미리 정해진 방식을 통해 Controller에게 행위에 대한 요청(delegate)과 데이터에 대한 요청(data source)을 할 수 있다.

> 구조적: 이 통신을 하기로 했을 때 일반적인 객체는 어떻게 컨트롤러와 대화를 할지 조금 먼저 생각해놓고 있어야 한다는 것

더 나아가 `targer-action`의 구조를 통해 사용자의 행위에 따라 필요한 함수를 호출할 수도 있다.
- target: 컨트롤러가 해야하는 건 자신에게 타겟을 만드는 것
- action: UIButton이나 다른 것들은 액션을 가지고 버튼을 누를때마다 타겟을 호출

---
layout: post
title: 오토레이아웃이란?
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 오토레이아웃이란?

아이폰 기종은 우리도 다 알다시피 iPhone4, iPhoneSE, iPhone8, iPhone8 Plus 그리고 iPhoneX등 다양한 사이즈와 화면비율로 출시되면서 사이즈의 구애를 받지 않고 시각적으로 동일한 화면을 구현해야 하는 데 이를 위한 가장 편리하고 권장되는 방법이 **오토레이아웃** 이다.

- 뷰의 제약 사항을 바탕으로 뷰 체계 내의 모든 뷰의 크기와 위치를 동적으로 계산
- 애플리케이션을 사용할 때 발생하는 외부 변경과 내부 변경에 동적으로 반응하는 사용자 인터페이스를 가능하게 함


### 외부변경(External Changes)

슈퍼뷰의 크기나 모양이 변경될 때 발생<br>
각각의 변화와 함께 사용가능한 공간을 가장 잘 사용할 수 있도록 뷰 체계의 레이아웃을 업데이트 해줘야 함

- 사용자가 아이패드의 분할뷰(Split View)를 사용하거나 사용하지 않는 경우
- 장치를 회전하는 경우
- 활성화콜(acitive call)과 오디오 녹음 바가 보여지거나 사라지는 경우
- 다른 크기의 클래스를 지원하기 원하는 경우
- 다른 크기의 스크린을 지원하기 원하는 경우

이러한 변경사항은 대부분은 실행 시간에 발생할 수 있으며 애플리케이션으로부터 동적인 응답을 요구한다. 다른 스크린 크기를 지원하는 것과 같은 것은 애플리케이션이 다른 환경에 적응하는 것을 나타낸다. 스크린 크기가 일반적으로 실행 시간에 변경되지 않는다고 하더라도, 적응형 인터페이스를 만들면 애플리케이션이 아이폰 4S, 아이폰 6 플러스, 또는 아이패드에서도 모두 동일하게 잘 작동할 수 있다. 오토레이아웃은 아이패드 내부 변경에서 슬라이드와 분할뷰를 지원하는 핵심 요소이기도 합니다.


### 내부변경(Internal Changes)

사용자 인터페이스의 뷰의 크기 또는 설정이 변경되었을 때 발생

- 애플리케이션 변경에 의해 콘텐츠가 보여지는 경우
- 애플리케이션이 국제화를 지원하는 경우
- 애플리케이션이 동적 타입을 지원하는 경우

애플리케이션 콘텐츠가 변경됐을 때, 새로운 콘텐츠는 예전과 다른 레이아웃을 필요 할 수 있다. 새로운 애플리케이션이 각각의 신문 기사의 크기에 기반을 둔 레이아웃을 조정할 필요가 있는 경우와 같이, 텍스트 또는 이미지를 보여주는 애플리케이션에서 일반적으로 발생하는 일이다. 이와 비슷하게, 사진 콜라주는 이미지 크기와 영상의 가로 세로의 비율을 다뤄야한다.



#### 오토레이아웃이 왜 필요할까?

오토레이아웃은 아래의 경우와 같이 인터페이스의 절대적인 좌표가 아닌 **동적으로 상대적인 좌표가 필요한 경우에 유용** 하다.

- 애플리케이션이 실행되는 iOS 기기의 스크린 화면의 크기가 다양한 경우
- 애플리케이션이 실행되는 iOS 기기의 스크린이 회전할 수 있는 경우
- 상태표시줄(Status Bar)에 전화 중임을 나타내는 액티브 콜(active call)과 오디오 녹음 중임을 나타내는 오디오 바가 보여지거나 사라지는 경우
- 애플리케이션의 콘텐츠가 동적으로 보여지는 경우
- 애플리케이션이 지역화(Localization)를 지원하는 경우
- 애플리케이션이 동적 타입을 지원하는 경우


#### 오토레이아웃 속성

오토레이아웃의 속성은 정렬 사각형을 기반으로 한다.

<center>
<figure>
<img src="/assets/post-img/swift/30.png" alt="" width="50%">
</figure>
</center>

- Width : 정렬 사각형의 너비
- Height : 정렬 사각형의 높이
- Top : 정렬 사각형의 상단
- Bottom : 정렬 사각형의 하단
- Baseline : 텍스트의 하단
- Horizontal : 수평
- Vertical : 수직
- Leading : 리딩, 텍스트를 읽을 때 시작 방향
- Trailing : 트레일링, 텍스트를 읽을 때 끝 방향
- CenterX : 수평 중심
- CenterY : 수직 중심


#### 안전 영역(Safe Area)

안전 영역은 **콘텐츠가 상태바, 내비게이션바, 툴바, 탭바를 가리는 것을 방지하는 영역** 입니다. 표준 시스템이 제공하는 뷰들은 자동으로 안전 영역 레이아웃 가이드를 준수하게 되어있다.
기존의 상/하단 레이아웃 가이드(Top/Bottom Layout Guide)를 대체하며, 하위 버전에도 호환하여 작동한다.

안전 영역은 iOS 11부터 사용할 수 있으며, iOS 11 미만의 버전에서는 상/하단 레이아웃 가이드를 사용한다.

<center>
<figure>
<img src="/assets/post-img/swift/31.png" alt="" width="50%">
</figure>
</center>


안전 영역 레이아웃 가이드는 UIView클래스의 var safeAreaLayoutGuide: UILayoutGuide로 접근할 수 있다.


#### 제약(Constraint)

제약은 뷰 스스로 또는 뷰 사이의 관계를 속성을 통하여 정의하며, 제약은 방정식으로 나타낼 수 있다.

<center>
<figure>
<img src="/assets/post-img/swift/32.png" alt="" width="50%">
</figure>
</center>

- Item1 : 방정식에 있는 첫 번째 아이템(B View)이며, 첫 번째 아이템은 반드시 뷰 또는 레이아웃 가이드
- Attribute1 : 첫번째 아이템에 대한 속성이며 이 경우, B View의 리딩
- Multiplier : 속성 2에 곱해지는 값이며, 이 경우 1.0이다.
- Item2 : 방정식에 있는 두 번째 아이템(A View)
- Attribute2 : 두번째 아이템에 대한 속성으로 이 경우, A View의 트레일링
- Constant : 두번째 아이템의 속성에 더해지는 상수 값

위의 예제 방정식의 제약을 해석하면 **'B View의 리딩은 A View의 트레일링의 1.0배에 8.0을 더한 위치'** 가 된다.


#### 고유 콘텐츠 크기(Intrinsic Content Size)

뷰의 고유 콘텐츠 크기는 뷰가 갖는 원래의 크기로, 예를 들어 레이블의 고유 콘텐츠 크기는 레이블의 텍스트의 크기고, 이미지의 고유 콘텐츠 크기는 이미지 자체의 크기이다.



#### 제약 우선도(Constraint Priorities)

오토레이아웃은 뷰의 고유 콘텐츠 크기를 각 크기에 대한 한 쌍의 제약을 사용하여 나타낸다. <br>
우선도가 높을수록 다른 제약보다 우선적으로 레이아웃에 적용하며, 같은 속성의 다른 제약과 경합하는 경우, 우선도가 낮은 제약은 무시된다.

- `콘텐츠 허깅 우선도(Content hugging priority)` : 콘텐츠 고유 사이즈보다 뷰가 커지지 않도록 제한.
  - 다른 제약사항보다 우선도가 높으면 뷰가 콘텐츠 사이즈보다 커지지 않음
- `콘텐츠 축소 방지 우선도(Content compression resistance priority)` : 콘텐츠 고유 사이즈보다 뷰가 작아지지 않도록 제한.
  - 다른 제약사항보다 우선도가 높으면 뷰가 콘텐츠 사이즈보다 작아지지 않음


<center>
<figure>
<img src="/assets/post-img/swift/33.png" alt="" width="50%">
</figure>
</center>


#### 레이아웃 마진

뷰에 콘텐츠 내용을 레이아웃할 때 사용하는 기본 간격(default spacing)

**레이아웃 마진 가이드(Layout Margins Guide)**: 레이아웃 마진에 따라 형성되는 사각의 프레임 영역


#### 앵커(Anchor)

오토레이아웃을 코딩으로 구현하여 제약(Constraint)을 만들기 위해 앵커(Anchor)를 사용



#### Layout Anchor 사용 예제

중앙에 버튼을 배치하고 버튼의 top anchor를 사용하여 레이블을 버튼의 상단으로부터 10만큼 떨어지도록 배치

1. 객체 라이브러리에서 버튼과 레이블을 추가해줍니다. 이제 앵커를 활용하여 제약을 만들어봅시다.


<center>
<figure>
<img src="/assets/post-img/swift/34.png" alt="" width="50%">
</figure>
</center>

2. `@IBOutlet`을 활용하여 인터페이스 빌더에서 ViewController.swift 파일로 버튼과 레이블을 연결해줍니다.


<center>
<figure>
<img src="/assets/post-img/swift/35.png" alt="" width="50%">
</figure>
</center>


3. 버튼을 중앙에 배치하기 위해 버튼의 수평과 수직의 중앙 앵커를 뷰 컨트롤러의 뷰의 중앙에 기준을 잡아준다. 생성된 제약을 적용하기 위해선 **isActive 프로퍼티의 값을 true로 설정** 해주면 됩니다.


<center>
<figure>
<img src="/assets/post-img/swift/36.png" alt="" width="50%">
</figure>
</center>


`translatesAutoresizingMaskIntoConstraints` : 오토레이아웃이 도입되기 전 뷰를 유연하게 표현할 수 있도록 오토리사이징 마스크를 사용하였는데, 오토레이아웃을 사용하게 되면 기존의 오토리사징 마스크가 가지고 있던 제약조건이 자동으로 추가되기 때문에 충돌하게 될 가능성이 발생한다. 그래서 **translatesAutoresizingMaskIntoConstraints의 값을 false로 지정** 한 뒤 오토레이아웃을 적용해준다. 참고로 인터페이스 빌더에서 오토레이아웃을 적용한 경우에는 자동으로 값이 false로 설정됩니다. (참조: [translatesAutoresizingMaskIntoConstraints](https://developer.apple.com/documentation/uikit/uiview/1622572-translatesautoresizingmaskintoco))


4. 레이블의 수평 중앙을 버튼의 수평 중앙 앵커를 기준으로 제약을 생성한 후, 레이블의 하단 앵커를 버튼의 상단 앵커로부터 10만큼의 거리를 두도록 한다.**(상단 앵커기준으로 위로의 거리는 부호가 - 라는 점을 주목)** 생성된 제약을 적용하기 위해 isActive 프로퍼티를 **true** 로 설정. 그림과 같이 레이블이 버튼의 상단에 자리 잡고 있는 것을 볼 수 있다.

속성에 곱해지는 multiplier를 활용 > 앵커를 활용하여 레이블의 너비가 버튼의 너비의 2배가 되도록 제약을 만들어 본다.

<center>
<figure>
<img src="/assets/post-img/swift/37.png" alt="" width="50%">
</figure>
</center>

5. 위의 코드를 추가하여 레이블의 너비가 버튼의 너비의 2배가 된 것을 확인

<center>
<figure>
<img src="/assets/post-img/swift/38.png" alt="" width="50%">
</figure>
</center>

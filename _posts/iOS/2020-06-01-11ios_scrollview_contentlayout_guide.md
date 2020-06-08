---
layout: post
title: Xcode11 새로워진 ScrollView 만들기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

프로젝트를 진행하면서 scroll view를 구현할 일이 생겼는데, 그냥 뷰컨트롤러 위에 스크롤뷰를 올리고 (0,0,0,0)을 하는데 적용은 되지않고 계속 빨간줄만 뜬다.

찾아보니 이번 Xcode 11 버전에서 인터페이스 빌더로 스크롤뷰를 만들때 contentLayoutGuide, frameLayoutGuide가 기본적으로 활성화 되도록 추가되었다고 한다.

## Xcode 11 Release Notes

#### Interface Builder

> Content and Frame Layout guides are supported for UIScrollView and can be enabled in the Size inspector for more control over your scrollable content. (29711618)

릴리즈 노트를 보면 content 및 frame 레이아웃 가이드는 UIScrollView에서 지원되며, 스크롤 가능한 컨텐츠를 보다 효과적으로 제어할 수 있도록 Size inspector에서 활성화할 수 있다고 나와있다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/30.png" alt="" width="50%">
</figure>
</center>

Size inspector에서 확인해보면 Content Layout Guides 체크박스가 추가되어있습니다. 스크롤뷰 생성시 기본적으로 활성화 되어있기 때문에 따로 체크할 필요는 없습니다.

### contentLayoutGuide

ScrollView의 변환되지 않은 컨텐츠 사각형을 기반으로 하는 레이아웃 가이드<br>
ScrollView의 컨텐츠 영역과 관련된 오토 레이아웃 제약 조건을 만드려면 이 레이아웃 가이드를 사용하면 된다


### frameLayoutGuide

ScrollView의 변환되지 않은 프레임 사각형을 기반으로 하는 레이아웃 가이드<br>
컨텐츠 사각형 반대로 ScrollView 자체의 프레임 사각형을 포함하는 오토 레이아웃 제약 조건을 만드려면 이 레이아웃 가이드를 사용하면 된다


## Interface Builder로 ScrollView 만들기

### ScrollView 추가

ScrollView를 추가하고 화살표를 눌러 ContentLayoutGuide와 FrameLayoutGuide가 포함되어있는걸 확인할 수 있다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/31.png" alt="" width="50%">
</figure>
</center>

가장 간단하게 스크롤뷰를 구현하는 방법은 size inspector에 체크되어있는 `content layout guids`를 해제하면 이전에 했던 방법처럼 적용이 가능해진다.


### ScrollView Constraints 설정

ScrollView를 추가했으니 알맞게 제약을 설정해준다.

우선 릴리즈된 xcode에서는 safe area에 맞춰 (0,0,0,0)을 해주면 아래와 같이 빨간줄이 뚝! 뜨게된다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/32.png" alt="" width="50%">
</figure>
</center>


### Content Layout Guide 설정

ScrollView안에 컨텐츠를 넣기 위한 상위 View 하나를 추가해준다.<br>
이때 View의 제약조건을 ScrollView에 걸지 않고 Content Layout Guide에 걸어준다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/33.png" alt="" width="50%">
</figure>
</center>

여전히 빨간줄이 떠있다.



### Frame Layout Guide 설정

이제 Frame Layout Guide 에 Equal Widths 나 Equal Heights 제약을 추가해 고정시킬 방향을 설정한다.

- Equal Widths는 가로길이를 Frame Layout Guide에 고정시켜 세로 스크롤이 필요할때 사용
- Equal Heights는 세로길이를 Frame Layout Guide에 고정시켜 가로 스크롤이 필요할때 사용


새롭게 릴리즈된 xcode를 살펴보았으나.. 나는 스크롤뷰에 있어서 사실 더 편한 건지는 잘 모르겠다.

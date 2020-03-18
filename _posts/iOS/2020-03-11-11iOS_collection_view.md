---
layout: post
title: CollectionView, CollectionView Cell
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 컬렉션뷰

컬렉션뷰는 유연하고 변경 가능한 레이아웃을 사용하여 데이터 아이템의 정렬된 세트를 표시하는 수단이다.

컬렉션뷰의 가장 일반적인 용도는 데이터 아이템을 그리드와 같은 형태로 표현한다. 더불어 다양한 방법으로 컬렉션뷰의 레이아웃을 사용자 정의할 수 있다.

<center>
<figure>
<img src="/assets/post-img/iOS/106.png" alt="" width="80%">
</figure>
</center>


### 컬렉션뷰의 구성요소

<center>
<figure>
<img src="/assets/post-img/iOS/107.png" alt="" width="80%">
</figure>
</center>

- 셀(Cell) : 컬렉션뷰의 주요 콘텐츠를 표시
  - 컬렉션뷰는 컬렉션뷰 데이터 소스 객체에서 표시할 셀에 대한 정보를 가져온다.
  - 각 셀은 UICollectionViewCell 클래스의 인스턴스 또는 UICollectionViewCell을 상속받은 클래스의 인스턴스
- 보충 뷰(Supplementary views) : 섹션에 대한 정보를 표시
  - 셀과 달리 보충 뷰는 필수는 아니며, 사용법과 배치 방식은 사용되는 레이아웃 객체가 제어한다 > 헤더와 푸터를 예로!
- 데코레이션 뷰(Decoration views) : 콘텐츠가 스크롤 되는 컬렉션뷰에 대한 배경을 꾸밀 때 사용
  - 레이아웃 객체는 데코레이션 뷰를 사용하여 커스텀 배경 모양을 구현할 수 있다
- 레이아웃 객체(Layout Object) : 레이아웃 객체는 컬렉션뷰 내의 아이템 배치 및 시각적 스타일을 결정
  - 컬렉션뷰 데이터 소스 객체가 뷰와 표시할 콘텐츠를 제공한다면, 레이아웃 객체는 크기, 위치 및 해당 뷰의 레이아웃과 관련된 특성들을 결정


#### 컬렉션뷰 구현을 이루기 위한 클래스 및 프로토콜

컬렉션뷰를 사용하여 콘텐츠를 화면에 표시하기 위해서 컬렉션뷰는 여러 가지 다른 객체들과 협력한다. 예를 들어, 컬렉션뷰는 컬렉션뷰 데이터 소스 객체로부터 표시할 콘텐츠의 정보를 얻어오고, 사용자와의 상호작용을 처리하기 위해 컬렉션뷰 델리게이트 객체를 사용한다

- 최상위 포함 및 관리 (Top level containment)
  - UICollectionView / UICollectionViewController : UICollectionView는 컬렉션뷰의 콘텐츠가 보이는 영역을 정의하며, UICollectionViewController는 컬렉션뷰를 관리하는 뷰 컨트롤러이다. UICollectionViewController는 선택적으로 사용 가능!
- 콘텐츠 관리
  - UICollectionViewDataSource protocol / UICollectionViewDelegate protocol : 데이터 소스 객체는 컬렉션뷰와 관련된 중요한 객체이며, 필수적으로 제공해야 한다. 데이터 소스는 컬렉션뷰의 콘텐츠를 관리하고 해당 콘텐츠를 표시하기 위한 뷰를 제공하며 컬렉션뷰 델리게이트 객체는 사용자와의 상호작용과 셀 강조 표시 및 선택 등을 관리한다.
- 표시(Presentation)
  - UICollectionReusableView / UICollectionViewCell : 컬렉션에 표시된 모든 뷰는 UICollectionReusableView 클래스의 인스턴스여야 한다. 이 클래스는 컬렉션뷰에서 사용 중인 뷰 재사용 메커니즘을 지원하며, 새로운 뷰를 만드는 대신, 뷰를 재사용하여 성능을 향상시킨다.
- 레이아웃(Layout)
  - UICollectionViewLayout / UICollectionViewLayoutAttribute / UICollectionViewUpdateItem : UICollectionViewLayout의 서브클래스는 레이아웃 객체라고 하며 컬렉션 뷰 내부의 셀 및 재사용 가능한 뷰의 위치, 크기 및 시각적 속성을 정의한다. UICollectionViewLayoutAttributes는 레이아웃 프로세스 중에 컬렉션뷰에 셀과 재사용가능한 뷰를 표시하는 위치와 방법을 알려주며, 레이아웃 객체 아이템이 삽입, 삭제, 혹은 컬렉션뷰 내에서 이동할 때마다 레이아웃 객체는 UICollectionViewUpdateItem 클래스의 인스턴스를 받는다.
- 플로우 레이아웃(Flowlayout)
  - UICollectionViewFlowLayout / UICollectionViewDelegatFlowLayout protocol : 그리드 혹은 다른 라인기반(lined-based) 레이아웃을 구현하는 데 사용된다. 클래스를 그대로 사용하거나 동적으로 커스터마이징할 수 있는 플로우 델리게이트 객체와 함께 사용할 수 있다.


#### 컬렉션뷰와 관련된 클래스 및 프로토콜

- UICollectionView : 사용자에게 보여질 컬렉션 형태의 뷰
- UICollectionViewCell : UICollectionView 인스턴스에 제공되는 데이터를 화면에 표시하는 역할을 담당
- UICollectionReusableView : 뷰 재사용 메커니즘을 지원
- UICollectionViewFlowLayout : 컬렉션뷰를 위한 디폴트 클래스로, 그리드 스타일로 셀들을 배치하도록 설계되어있다. scrollDirection 프로퍼티를 통해 수평 및 수직 스크롤을 지원
- UICollectionViewLayoutAttributes : 컬렉션뷰 내의 지정된 아이템의 레이아웃 관련 속성을 관리
- UICollectionViewDataSource 프로토콜 : 컬렉션뷰에 필요한 데이터 및 뷰를 제공하기 위한 기능을 정의한 프로토콜
- UICollectionViewDelegate 프로토콜 : 컬렉션뷰에서 아이템의 선택 및 강조 표시를 관리하고 해당 아이템에 대한 작업을 수행할 수 있는 기능을 정의한 프로토콜
- UICollectionViewDelegateFlowLayout 프로토콜 : UICollectionViewLayout 객체와 함께 그리드 기반 레이아웃을 구현하기 위한 기능을 정의한 프로토콜



<hr>
## 컬렉션뷰 셀

컬렉션뷰의 셀은 냉장고 속에 있는 반찬통으로 생각할 수 있다. 컬렉션뷰라는 냉장고가 있고, 냉장고 안에는 실제 반찬(콘텐츠)을 담고 있는 컬렉션뷰 셀이라는 반찬통이 있다고 생각할 수 있다.

- 컬렉션뷰 셀은 데이터 아이템을 화면에 표시
- 하나의 셀은 하나의 데이터 아이템을 화면에 표시
- 컬렉션뷰 셀은 두 개의 배경을 표시하는 뷰와 하나의 콘텐츠를 표시하는 뷰로 구성되어 있다. 두 개의 배경뷰는 셀이 선택되었을 때 사용자에게 시각적인 표현을 제공하기 위해 사용
- 셀의 레이아웃은 컬렉션뷰의 레이아웃 객체에 의해 관리
- 컬렉션뷰 셀은 뷰의 재사용 메커니즘을 지원
- 일반적으로 컬렉션뷰 셀 클래스의 인스턴스는 직접 생성하지 않는다.
  - 대신 특정 셀의 하위 클래스를 컬렉션뷰 객체에 등록한 후, 컬렉션뷰 셀 클래스의 새로운 인스턴스가 필요할 때, 컬렉션의 dequeueReusableCell(withReuseIdentifier:for:) 메서드를 호출
  - 스토리보드를 사용하여 셀을 구성하면 컬렉션뷰에 따로 셀 클래스를 등록할 필요는 없습니다.


### UICollectionViewCell 클래스

컬렉션뷰 셀의 구성요소 관련 프로퍼티

- var contentView: UIView : 셀의 콘텐츠를 표시하는 뷰
- var backgroundView: UIView? : 셀의 배경을 나타내는 뷰
  - 이 프로퍼티는 셀이 처음 로드되었을 경우와 셀이 강조 표시되지 않거나 선택되지 않을 때 항상 기본 배경의 역할을 한다
- var selectedBackgroundView: UIView? : 셀이 선택되었을 때 배경뷰 위에 표시되는 뷰
  - 이 프로퍼티는 셀이 강조 표시되거나 선택될 때마다 기본 배경 뷰인 backgroundView를 대체하여 표시


#### 컬렉션뷰 셀의 상태 관련 프로퍼티

- var isSelected: Bool : 셀이 선택되었는지를 나타냄 > 셀이 선택되어있지 않다면 이 프로퍼티의 값은 false
- var isHighlighted: Bool : 셀의 하이라이트 상태를 나타냄 > 하이라이트 되어있지 않다면 기본 값은 false


#### 컬렉션뷰 셀의 드래그 상태 관련 메서드

```swift
// 셀의 드래그 상태가 변경되면 호출
func dragStateDidChange(_:)
```

드래그의 상태는 UICollectionViewCellDragState의 열거형으로 표현되고 none, lifting, dragging의 3가지 상태를 갖는다.


## 컬렉션뷰 셀 vs 테이블뷰 셀

컬렉션뷰를 학습하면서 앞서 배웠던 테이블뷰와 비슷한 점이 많지 않았나요? 그렇다면 컬렉션뷰 셀과 테이블뷰 셀에는 어떠한 차이점이 있는지 알아볼까요?

테이블뷰 셀의 구조는 콘텐츠 영역과 액세서리뷰 영역으로 나뉘었지만, 컬렉션뷰 셀은 배경뷰와 실제 콘텐츠를 나타내는 콘텐츠뷰로 나뉘었습니다.
테이블뷰 셀은 기본으로 제공되는 특정 스타일을 적용할 수 있지만 컬렉션뷰 셀은 특정한 스타일이 따로 없습니다.
테이블뷰 셀은 목록형태로만 레이아웃 되지만, 컬렉션뷰 셀은 다양한 레이아웃을 지원합니다.

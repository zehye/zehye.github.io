---
layout: post
title: Table View
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 테이블뷰(Table View)란?

테이블뷰는 iOS 애플리케이션에서 많이 활용하는 **사용자 인터페이스** 이다. <br>
테이블뷰는 **리스트** 형태를 지니고 있으며 **스크롤이 가능** 해 많은 정보를 보여 줄 수 있다.



### 테이블뷰 기본 형태

- 테이블뷰는 하나의 열(column)과 여러 줄의 행(row)을 지니며, 수직으로만 스크롤 가능
- 각 행은 하나의 셀(cell)에 대응
- 섹션(section)을 이용해 행을 시각적으로 나눌 수 있다
- 헤더(header)와 푸터(footer)에 이미지나 텍스트를 추가해 추가 정보를 보여줄 수 있다


<center>
<figure>
<img src="/assets/post-img/iOS/25.png" alt="" width="50%">
</figure>
</center>

<center>
<figure>
<img src="/assets/post-img/iOS/26.png" alt="" width="50%">
</figure>
</center>

### 테이블뷰 스타일

테이블뷰는 크게 두 가지 스타일(일반, 그룹)로 나뉜다.

- 일반 테이블뷰(Plain TableView)
  - 더 이상 나뉘지 않는 연속적인 행의 리스트 형태
  - 하나 이상의 섹션을 가질 수 있으며, 각 섹션은 여러 개의 행을 지닐 수 있다
  - 각 섹션은 헤더 혹은 푸터를 옵션으로 지닐 수 있다
  - 색인을 이용한 빠른 탐색을 하거나 옵션을 선택할 때 용이
- 그룹 테이블뷰(Grouped TableView):
  - 섹션을 기준으로 그룹화되어있는 리스트 형태
  - 하나 이상의 섹션을 가질 수 있으며, 각 섹션은 여러 개의 행을 지닐 수 있다
  - 각 섹션은 헤더 혹은 푸터를 옵션으로 지닐 수 있다
  - 정보를 특정 기준에 따라 개념적으로 구분할 때 적합
  - 사용자가 정보를 빠르게 이해하는 데 도움이 된다

<center>
<figure>
<img src="/assets/post-img/iOS/27.png" alt="" width="50%">
</figure>
</center>

<center>
<figure>
<img src="/assets/post-img/iOS/28.png" alt="" width="50%">
</figure>
</center>


### 테이블뷰 생성

테이블뷰를 생성하고 관리하는 좋은 방법은 **스토리보드에서 커스텀 UITableViewController 클래스의 객체를 이용** 하는 것이다(필요에 따라서 소스코드로 테이블뷰를 생성하는 것도 물론 가능하다) 스토리보드에서 테이블뷰의 특성을 지정할 때, 동적 프로토타입(dynamic prototypes) 혹은 정적 셀(static cells) 중 하나를 선택할 수 있다. 새로운 테이블뷰를 생성할 때 **기본 설정 값은 동적 프로토타입** 이다.

- 동적 프로토타입(Dynamic Prototypes)
  - 셀 하나를 디자인해 이를 다른 셀의 템플릿으로 사용하는 방식
  - 같은 레이아웃의 셀을 여러 개 이용해 정보를 표시할 경우
  - 데이터 소스(UITableViewDataSource) 인스턴스에 의해 콘텐츠를 관리하며, 셀의 개수가 상황에 따라 변하는 경우에 사용
- 정적 셀(Static Cells)
  - 고유의 레이아웃과 고정된 수의 행을 가지는 테이블뷰에 사용
  - 테이블뷰를 디자인하는 시점에 테이블의 형태와 셀의 개수가 정해져 있는 경우 사용
  - 셀의 개수가 변하지 않음


### 테이블뷰 구성요소

테이블뷰를 구성하기 위해 꼭 알아야 하는 개념에는 셀(cell), 델리게이트(delegate) 그리고 데이터 소스(data source)가 있습니다


<hr>

## In Apple Docs...


### UITableView

[]()
#### Declaration

```swift
class UITableView: UIScrollView
```

iOS의 테이블뷰에는 세로 스크롤 컨텐츠의 단일 열이 행으로 나누어 표시된다. 각 행에는 앱 콘텐츠가 하나씩 포함되어 있다. 하나의 긴 행 목록을 표시하도록 테이블을 구성하거나 컨텐츠를 보다 쉽게 ​​탐색 할 수 있도록 관련 행을 섹션으로 그룹화 할 수 있다.

테이블은 일반적으로 데이터가 고도로 구조화되거나 체계적으로 구성된 앱에서 사용된다. 계층 적 데이터를 포함하는 앱은 종종 탐색 뷰 컨트롤러와 함께 테이블을 사용하여 여러 수준의 계층 구조 간 탐색을 용이하게한다. 예를 들어 설정 앱은 테이블과 탐색 컨트롤러를 사용하여 시스템 설정을 구성한다. UITableView는 테이블의 기본 모양을 관리하지만 앱은 실제 내용을 표시하는 셀 (UITableViewCell 객체)을 제공한다. 표준 셀 구성에는 텍스트와 이미지의 간단한 조합이 표시되지만 원하는 내용을 표시하는 사용자 정의 셀을 정의 할 수 있다. 헤더와 푸터를 제공하여 셀 그룹에 대한 추가 정보를 제공 할 수도 있다.

테이블 뷰는 데이터 중심이며 일반적으로 제공하는 데이터 소스 개체에서 데이터를 가져온다. 데이터 소스 개체는 앱의 데이터를 관리하며 테이블의 셀을 만들고 구성한다. 테이블의 내용이 변경되지 않으면 스토리 보드 파일에서 해당 내용을 대신 구성 할 수 있다.


### Table Views

테이블뷰에는 세로 스크롤 컨텐츠의 단일 열이 행과 섹션으로 나누어 표시된다. 테이블의 각 행에는 앱과 관련된 단일 정보가 표시되며 섹션을 사용하면 관련 행을 그룹화 할 수 있다.

테이블 뷰는 다음을 포함하여 여러 다른 객체 간의 협업입니다.
- Cells: 셀은 컨텐츠의 시각적 표현을 제공
  - UIKit에서 제공하는 기본 셀을 사용하거나 앱의 요구에 맞게 사용자 정의 셀을 정의 할 수 있다
- Table View Controller: 일반적으로 UITableViewController 객체를 사용하여 테이블 뷰를 관리
  - 다른 뷰 컨트롤러도 사용할 수 있지만 일부 테이블 관련 기능이 작동하려면 테이블 뷰 컨트롤러가 필요
- Data Source object: 이 객체는 UITableViewDataSource 프로토콜을 채택하고 테이블에 대한 데이터를 제공
- Delegate object: 이 개체는 UITableViewDelegate 프로토콜을 채택하고 테이블 내용과의 사용자 상호 작용을 관리


### Table

[Table apple doce](https://developer.apple.com/design/human-interface-guidelines/ios/views/tables/)

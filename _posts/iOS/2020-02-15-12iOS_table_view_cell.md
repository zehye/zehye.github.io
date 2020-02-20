---
layout: post
title: TableView Cell
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 테이블뷰 셀

테이블뷰 셀(TableView Cell)은 테이블뷰를 이루는 개별적인 행(row)으로, UITableViewCell 클래스를 상속받는다. UITableViewCell 클래스에 정의된 표준 스타일을 활용해 문자열 혹은 이미지를 제공하는 셀을 생성할 수 있으며, 커스텀 서브뷰를 올려 다양한 시각적 모습을 나타낼 수 있다.


### 테이블뷰 셀의 구조

기본적으로 테이블뷰 셀은 아래 이미지와 같이 크게 콘텐츠 영역과 액세서리뷰 영역으로 구조가 나뉩니다.

<center>
<figure>
<img src="/assets/post-img/iOS/29.png" alt="" width="50%">
</figure>
</center>

- 콘텐츠 영역: 셀의 왼쪽 부분에는 주로 문자열, 이미지 혹은 고유 식별자 등이 입력
- 액세서리뷰 영역: 셀의 오른쪽 작은 부분은 액세서리뷰로 상세보기, 재정렬, 스위치 등과 같은 컨트롤 객체가 위치


<center>
<figure>
<img src="/assets/post-img/iOS/30.png" alt="" width="50%">
</figure>
</center>


테이블뷰를 편집 모드(Editing Mode)로 전환하면 아래와 같은 구조로 바뀐다


<center>
<figure>
<img src="/assets/post-img/iOS/31.png" alt="" width="50%">
</figure>
</center>


편집 컨트롤은 삭제 컨트롤(빨간 원 안의 마이너스 기호) 또는 추가 컨트롤(녹색 원 안의 플러스 기호) 중 하나가 될 수 있으며 재정렬이 가능한 경우, 재정렬 컨트롤이 액세서리뷰에 나타난다. 재정렬 컨트롤을 눌러 셀을 드래그하면 위아래로 순서를 변경할 수 있다.

<center>
<figure>
<img src="/assets/post-img/iOS/32.png" alt="" width="50%">
</figure>
</center>


### 테이블뷰 셀의 기본 기능

UITableViewCell 클래스를 상속받는 기본 테이블뷰 셀은 표준 스타일을 이용할 수 있다. 표준 스타일의 콘텐츠 영역은 한 개 이상의 문자열 그리고 이미지를 지닐 수 있으며, 이미지가 오른쪽으로 확장됨에 따라 문자열이 오른쪽으로 밀려난다.

<center>
<figure>
<img src="/assets/post-img/iOS/33.png" alt="" width="50%">
</figure>
</center>

- UITableViewCell 클래스는 셀 콘텐츠에 세 가지 프로퍼티가 정의되어 있음
- textLabel: UILabel: 주제목 레이블
- detailTextLabel: UILabel: 추가 세부 사항 표시를 위한 부제목 레이블
- imageView: UIImageView: 이미지 표시를 위한 이미지뷰

<center>
<figure>
<img src="/assets/post-img/iOS/34.png" alt="" width="50%">
</figure>
</center>


### 커스텀 테이블뷰 셀

UITableViewCell 클래스에서 제공하는 표준 스타일 셀을 이용해 이미지와 문자열을 표현하고 글꼴 및 색상 등을 수정할 수 있지만, 기본 형태를 벗어나 다양한 애플리케이션의 요구를 충족시키기 위해 셀을 커스텀 할 수 있다. 셀을 커스텀 하면 이미지를 텍스트 오른쪽에 위치시키는 등 원하는 시각적 형태를 만들 수 있다.

셀을 커스텀 하는 방법에는 크게 두 가지 방법이 있는데, 스토리보드를 이용하거나 코드로 구현할 수 있다.

- 셀의 콘텐츠뷰에 서브뷰 추가하기
- UITableViewCell의 커스텀 서브클래스 만들기

[참고] UITableViewCell의 서브클래스를 이용해 커스텀 이미지뷰를 생성하는 경우, 이미지뷰의 변수명을 imageView로 명명하면 기본 이미지뷰 프로퍼티와 변수명이 같아 원하는 대로 동작하지 않을 수 있으니 반드시 커스텀 이미지뷰의 변수명은 다르게 지어주세요(예. detailImageView, thumbnailImageView, profileImageView). textLabel, detailLabel, accessoryView 등의 기본 프로퍼티 이름 모두 마찬가지입니다.


<hr>

## In Apple Docs...


### UITableViewCell

[참고한 애플 문서-UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)

#### Declaration

```swift
class UITableViewCell : UIView
```

UITableViewCell 객체는 단일 테이블 행의 내용을 관리하는 특수한 유형의 뷰입니다. 주로 셀을 사용하여 앱의 사용자 정의 컨텐츠를 구성하고 표시하지만 UITableViewCell은 다음과 같은 테이블 관련 동작을 지원하기위한 특정 사용자 정의를 제공합니다.

셀에 선택 또는 강조 색상을 적용합니다. 세부 사항 또는 공개 제어와 같은 표준 액세서리보기 추가 셀을 편집 가능한 상태로 만들기 셀 내용을 들여 쓰기하여 테이블에 시각적 계층을 만듭니다.

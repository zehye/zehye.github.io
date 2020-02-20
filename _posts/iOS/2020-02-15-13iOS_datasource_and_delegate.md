---
layout: post
title: Data Source and Delegate
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

테이블뷰에 원하는 정보를 표시하고, 사용자 선택에 적절히 반응하는 테이블뷰 구현을 위해 꼭 필요한 데이터 소스와 델리게이트에 대해 알아보도록 하자.


## 테이블뷰 데이터 소스와 델리게이트

UITableView 객체는 데이터 소스와 델리게이트가 없다면 정상적으로 동작하기 어려우므로 두 객체가 꼭 필요하다.

MVC(Model-View-Controller) 프로그래밍 디자인 패턴에 따라 데이터 소스는 애플리케이션의 데이터 모델(M)과 관련되어 있으며, 델리게이트는 테이블뷰의 모양과 동작을 관리하기에 컨트롤러(C)의 역할에 가깝다. 테이블뷰는 뷰(V)의 역할을 합니다. 데이터 소스와 델리게이트를 통해 테이블뷰를 매우 유연하게 만들 수 있다.


### 데이터 소스

- 테이블뷰 데이터 소스 객체는 UITableViewDataSource 프로토콜을 채택한다
- 데이터 소스는 테이블 뷰를 생성하고 수정하는데 필요한 정보를 테이블뷰 객체에 제공
- 데이터 소스는 데이터 모델의 델리게이트로, 테이블뷰의 시각적 모양에 대한 최소한의 정보를 제공
- UITableView 객체에 섹션의 수와 행의 수를 알려주며, 행의 삽입, 삭제 및 재정렬하는 기능을 선택적으로 구현
- UITableViewDataSource 프로토콜의 주요 메서드는 아래와 같다. 이 중 `@required`로 선언된 두 가지 메서드는 UITableViewDataSource 프로토콜을 채택한 타입에 필수로 구현해야 한다

```swift
@required
// 특정 위치에 표시할 셀을 요청하는 메서드
func tableView(UITableView, cellForRowAt: IndexPath)

// 각 섹션에 표시할 행의 개수를 묻는 메서드
func tableView(UITableView, numberOfRowsInSection: Int)

@optional
// 테이블뷰의 총 섹션 개수를 묻는 메서드
func numberOfSections(in: UITableView)

// 특정 섹션의 헤더 혹은 푸터 타이틀을 묻는 메서드
func tableView(UITableView, titleForHeaderInSection: Int)
func tableView(UITableView, titleForFooterInSection: Int)

// 특정 위치의 행을 삭제 또는 추가 요청하는 메서드
func tableView(UITableView, commit: UITableViewCellEditingStyle, forRowAt: IndexPath)

// 특정 위치의 행이 편집 가능한지 묻는 메서드
func tableView(UITableView, canEditRowAt: IndexPath)

// 특정 위치의 행을 재정렬 할 수 있는지 묻는 메서드
func tableView(UITableView, canMoveRowAt: IndexPath)

// 특정 위치의 행을 다른 위치로 옮기는 메서드
func tableView(UITableView, moveRowAt: IndexPath, to: IndexPath)
```


### 델리게이트

- 테이블뷰 델리게이트 객체는 UITableViewDelegate 프로토콜을 채택
- 델리게이트는 테이블뷰의 시각적인 부분 수정, 행의 선택 관리, 액세서리뷰 지원 그리고 테이블뷰의 개별 행 편집을 도와줌
- 델리게이트 메서드를 활용하면 테이블뷰의 세세한 부분을 조정할 수 있다
- UITableViewDelegate 프로토콜의 주요 메서드는 아래와 같으며 이 중 필수로 구현해야 하는 메서드는 없다.

```swift
// 특정 위치 행의 높이를 묻는 메서드
func tableView(UITableView, heightForRowAt: IndexPath)

// 특정 위치 행의 들여쓰기 수준을 묻는 메서드
func tableView(UITableView, indentationLevelForRowAt: IndexPath)

// 지정된 행이 선택되었음을 알리는 메서드
func tableView(UITableView, didSelectRowAt: IndexPath)

// 지정된 행의 선택이 해제되었음을 알리는 메서드
func tableView(UITableView, didDeselectRowAt: IndexPath)

// 특정 섹션의 헤더뷰 또는 푸터뷰를 요청하는 메서드
func tableView(UITableView, viewForHeaderInSection: Int)
func tableView(UITableView, viewForFooterInSection: Int)

// 특정 섹션의 헤더뷰 또는 푸터뷰의 높이를 물어보는 메서드
func tableView(UITableView, heightForHeaderInSection: Int)
func tableView(UITableView, heightForFooterInSection: Int)

// 테이블뷰가 편집모드에 들어갔음을 알리는 메서드
func tableView(UITableView, willBeginEditingRowAt: IndexPath)

// 테이블뷰가 편집모드에서 빠져나왔음을 알리는 메서드
func tableView(UITableView, didEndEditingRowAt: IndexPath?)
```


<hr>

## In Apple Docs...


### UITableViewDataSource

[참고한 애플 문서-UITableViewDataSource](https://developer.apple.com/documentation/uikit/uitableviewdatasource)

#### Declaration

```swift
protocol UITableViewDataSource
```

테이블 뷰는 데이터 표현 만 관리하며 데이터 자체를 관리하지 않는다. 데이터를 관리하기 위해 데이터 소스 오브젝트, 즉 UITableViewDataSource 프로토콜을 구현하는 오브젝트를 테이블에 제공한다. 데이터 소스 개체는 테이블의 데이터 관련 요청에 응답하며, 테이블의 데이터를 직접 관리하거나 앱의 다른 부분과 조정하여 해당 데이터를 관리한다.

- 테이블의 섹션 및 행 개수를 보고
- 테이블의 각 행에 셀을 제공
- 섹션 머리글과 바닥 글에 제목을 제공
<!-- - 테이블의 인덱스 구성(있는 경우) 기본 데이터를 변경해야하는 사용자 또는 테이블 시작 업데이트에 응답 -->

```swift
// Return the number of rows for the table.     
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
   return 0
}

// Provide a cell object for each row.
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
   // Fetch a cell of the appropriate type.
   let cell = tableView.dequeueReusableCell(withIdentifier: "cellTypeIdentifier", for: indexPath)

   // Configure the cell’s contents.
   cell.textLabel!.text = "Cell text"

   return cell
}
```


### UITableViewDelegate

[참고한 애플 문서-UITableViewDelegate](https://developer.apple.com/documentation/uikit/uitableviewdelegate)

#### Declaration

```swift
protocol UITableViewDelegate
```

- 사용자 정의 머리글 및 바닥 글보기를 만들고 관리
- 행, 머리글 및 바닥 글에 대한 사용자 지정 높이를 지정
- 더 나은 스크롤링 지원을 위해 높이 추정치를 제공
- 행 내용을 들여 쓰고 행 선택에 응답
- 스와이프 및 테이블 행의 다른 작업에 응답 > 테이블 내용 편집을 지원
- 테이블 뷰는 NSIndexPath 객체를 사용하여 행과 섹션을 지정

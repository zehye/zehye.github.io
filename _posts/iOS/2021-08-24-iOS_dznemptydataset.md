---
layout: post
title: iOS DZNEmptyDateSet 라이브러리 사용해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## DZNEmptyDataSet

테이블 뷰나 컬렉션 뷰 등에 데이터가 없을 때, 보여줄 수 있는 심플한 화면을 손쉽게 관리할 수 있는 라이브러리


### 사용이유

1. 데이터가 없을때의 단순히 흰색 화면을 피학 화면이 비어있는 이유를 사용자에게 전달 가능
2. 일관성 유지 및 사용자 경험 개선 제공
3. 브랜드 존재감 제공



### Features

- UITableView, UICollectionView와 호환이 된다. 뿐만 아니라 UISearchDisplayController, UIScrollView와도 호환가능
- 이미지, 제목 및 설명 레이블, 버튼을 표시함으로써 레이아웃 및 모양의 다양성을 제공
- NSAttributedString 또한 사용가능
- 오토레이아웃을 사용함으로써 회전과 함께 콘텐츠를 자동으로 뷰의 중앙에 배치시켜준다. > 수직, 수평 정렬을 허용
- 배경색상 또한 사용자 정의 가능
- 테이블뷰 탭 제스처 허용
- 스토리보드와 호환 가능
- iOS6, tcOS9 이상부터 호환가능, iPhone, iPad, Apple TV와 호환 가능

해당 라이브러리는 UITableView, UICollectionView 클래스를 확장할 필요 없는 방식으로 설계되어있다. <br>
UITableViewController, UICollectionViewController를 사용할 때 여전히 작동이 가능하다.

**DZNEmptyDataSetDelegate, DZNEmptyDataSetSource** 만 준수한다면 애플리케이션의 내용과 빈 상태의 모양을 완전히 사용자 지정 가능하다.




## 사용해보기

```swift
pod 'DZNEmptyDateSet'
```

### viewcontroller

```swift
class ViewController: UIViewController {
  func viewDidLoad() {
    self.viewDidLoad()

    self.tableView.emptyDataSetSource = self
    self.tableView.emptyDataSetDelegate = self
  }
}

// MARK: DZNEmptyDataSetDelegate
extension ViewController: DZNEmptyDataSetDelegate {
    // 스크롤 권한 요청 > default는 false
    func emptyDataSetShouldAllowScroll(_ scrollView: UIScrollView!) -> Bool {
        return true/false
    }
}

// MARK: DZNEmptyDataSetSource
extension ViewController: DZNEmptyDataSetSource {
    // 비어있는 상태의 이미지 설정
    func image(forEmptyDataSet scrollView: UIScrollView!) -> UIImage! {
        return
    }
    // 비어있는 상태의 제목 설정
    func title(forEmptyDataSet scrollView: UIScrollView!) -> NSAttributedString! {
        return
    }
}
```

---
layout: post
title: iOS tableView delegate와 collectionView delegate(필요한 내용 추가합니다)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## tableView

### setHighlighted

셀의 강조 표시된 상태를 설정하고 선택적으로 상태 간 전환 애니메이션을 실행

```swift
func setHighlighted(_ highlighted: Bool, animated: Bool)
```

- highlighted: 셀을 강조 표시로 설정하려면 true, 아니면 false > default: false


### heightForHeaderInSection

특정 섹션의 헤더뷰 높이 지정

```swift
func tableView(_ tableView: UITableView, heightForHeaderInSection section: Int)
```

### heightForFooterInSection

특정 섹션의 푸터뷰 높이 지정

```swift
func tableView(_ tableView: UITableView, heightForFooterInSection section: Int)
```

### viewForHeaderInSection

특정 섹션의 헤더뷰 요청

```swift
func tableView(_ tableView: UITableView, viewForHeaderInSection section: Int)
```

### viewForFooterInSection

특정 섹션의 푸터뷰 요청

```swift
func tableView(_ tableView: UITableView, viewForFooterInSection section: Int)
```


## collectionView

### scrollToItem

지정된 항목이 표시될 때까지 collectionView 스크롤

```swift
func scrollToItem(at indexPath: IndexPath, at scrollPosition: UICollectionView.ScrollPosition, animated: Bool)
```

- indexPath: 스크롤할 뷰 항목의 인덱스 경로
- scrollPosition: 스크롤 완료될 때 항목이 배치되어야 하는 위치
- animated: 스크롤 동작 적용하려면 true, 스크롤 뷰의 내용을 즉지 조정하려면 false


### scrollViewWillEndDragging

스크롤 뷰의 드래그가 끝나기 직전에 델리게이트에 알림

```swift
optional func scrollViewWillEndDragging(_ scrollView: UIScrollView, withVelocity velocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>)
```



### showsHorizontalScrollIndicator

가로 스크롤 바 표시 여부를 제어하는 부울 값

```swift
var showsHorizontalScrollIndicator: Bool { get set }
```


### isPagingEnabled

페이징을 사용할 수 있는 여부를 결정하는 부울 값

```swift
var isPagingEnabled: Bool { get set }
```

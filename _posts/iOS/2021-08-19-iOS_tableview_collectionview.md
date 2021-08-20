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

쉽게말하면 셀을 터치했을때 화면의 섹이 살짝 바뀌는것을 볼수있다.<br>
사용자가 셀을 선택했는지 안했는지를 확인해볼 수 있음. > 사용자 관점에서 좀더 편리하다는 장점이 있다.<br>
개발자적 관점에서는 무엇을 터치했을때 에러가 발생했는지 조금더 직관적으로 볼 수 있다는 장점이 있을 듯 하다....


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

위 두개를 보통 사용하는건 이러한 예시가 있다.

테이블뷰의 타입을 그룹으로 했을때 기본적으로 섹션의 자동 높이가 설정되어 뷰에 나타나게 된다.<br>
그러나 나는 그 섹션의 높이가 마음에 안들거나 조절을 해야하는 경우가 있을텐데, 그때 이 delegate를 사용해준다.


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

[예제 블로그](https://velog.io/@dvhuni/Carousel-CollectionView-%EB%A7%8C%EB%93%A4%EC%96%B4%EB%B3%B4%EA%B8%B0)를 통해 확인해보면 <br>
스크롤뷰가 스크롤 되는 동안 계속 호출되는 delegate라고 한다.



### scrollViewWillEndDragging

스크롤 뷰의 드래그가 끝나기 직전에 델리게이트에 알림

```swift
optional func scrollViewWillEndDragging(_ scrollView: UIScrollView, withVelocity velocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>)
```

간단한 예시로 생각해보자.

테이블 뷰 안에 컬렉션 뷰가 있다고 생각해보자. <br>
테이블 뷰 중간에 컬렉션 뷰가 있었고, 컬렉션 뷰의 셀을 스크롤 해놓은 상태에서 > index가 0이 아닌상태에서 <br>
테이블뷰를 아래로 스크롤했다고 생각해보자.

이때 다시 해당 컬렉션뷰로 돌아갔을때, 컬렉션 뷰의 컨텐트 인덱스는 과연 어디에 위치해 있을까?

이를 정해주는 delegate이다.

말 그대로 사용자가 스크롤을 하고 스크린과 손이 떨어졌을때 호출되는 메소드이다.  <br>
이를 통해 스크롤할 때 컨텐트의 위치를 조정하여 페이징되는 효과를 낼 수 있다.


[예제 블로그](https://jintaewoo.tistory.com/33) < 요 블로그 예제를 보면 이해가 쉬울 것 같다.


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

---
layout: post
title: UICollectionView Flow Layout을 사용해 셀 크기를 조절해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## UICollectionViewFlowLayout

UICollectionViewFlowLayout 클래스를 사용하면 컬렉션뷰의 셀을 원하는 형태로 정렬할 수 있다. 플로우 레이아웃은 레이아웃 객체가 셀을 선형 경로에 배치하고 최대한 이 행을 따라 많은 셀을 채우는것을 의미한다. 현재 행에서 레이아웃 객체의 공간이 부족하면 새로운 행을 생성하고 거기에서 레이아웃 프로세스를 계속 진행한다.

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view.

    let flowLayout: UICollectionViewFlowLayout  // 이 타입의
    flowLayout = UICollectionViewFlowLayout()  // 인스턴스 생성
    // section의 inset을 없애고
    flowLayout.sectionInset = UIEdgeInsets.zero
    // 아이템간의 거리
    flowLayout.minimumInteritemSpacing = 10
    // 줄 간의 거리 (단위는 포인트)
    flowLayout.minimumLineSpacing = 10

    let halfWidth: CGFloat = UIScreen.main.bounds.width / 2.0
    // 오토레이아웃에 따라 사이즈는 가변적이기 때문에 정도값을 알려주는 것
    flowLayout.estimatedItemSize = CGSize(width: halfWidth - 30, height: 90)

    self.collectionView.collectionViewLayout = flowLayout
 }
```

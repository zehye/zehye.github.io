---
layout: post
title: iOS CollectionView Cell Dynamic Size 지정해주기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 컬렉션뷰 셀의 사이즈를 동적으로 조절하고 싶다면?

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/34.jpeg" alt="" width="80%">
</figure>
</center>

위 사진같이 셀을 동적으로 사이징 처리 해주고 싶다면?

구글링해보면 너무 어렵게만 설명되어있다.<br>
사실 별거 없다.


```swift
@IBOutlet weak var collectionView: UICollectionView!

func initUI() {
        self.collectionView.delegate = self
        self.collectionView.dataSource = self
        let layout = UICollectionViewFlowLayout()
        layout.estimatedItemSize = UICollectionViewFlowLayout.automaticSize
        self.collectionView.collectionViewLayout = layout
}
```

이렇게 해주면 셀 안의 데이터 크기에 맞게 알아서 사이징이 된다!

---
layout: post
title: 간단한 컬렉션 뷰 생성 후 동적으로 데이터 추가해보기(실습)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 간단한 컬렉션 뷰 생성

우선 스토리보드에 컬렉션 뷰를 생성해주고 해당 컬렉션 뷰에 레이블 두개를 올려보자.

<center>
<figure>
<img src="/assets/post-img/iOS/88.png" alt="" width="80%">
</figure>
</center>

레이블 적용한 오토레이아웃에서 참고할 점은 아래와 같다

<center>
<figure>
<img src="/assets/post-img/iOS/89.png" alt="" width="80%">
</figure>
</center>

그리고 컬렉션 뷰 셀에 해당하는 클래스를 만들어준다.

```swift
import UIKit

class FriendsCollectionViewCell: UICollectionViewCell {
    @IBOutlet var nameAgeLabel: UILabel!
    @IBOutlet var adressLadel: UILabel!
}
```

그리고 뷰 컨트롤러로 돌아와 아래 코드를 작성해준다.

```swift
import UIKit

class ViewController: UIViewController, UICollectionViewDataSource {

    // 임의로 지정해준 셀의 갯수
    var numberObCell: Int = 10
    let cellIdentifier: String = "cell"

    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {  // 10개의 셀이 생길것이다.
        return self.numberObCell

    }

    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell: UICollectionViewCell = collectionView.dequeueReusableCell(withReuseIdentifier: self.cellIdentifier, for: indexPath)
        return cell
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }
}
```

## 동적으로 데이터 추가해보기

이제 셀을 클릭하면 자동으로 셀이 생성되도록 만들어볼 것이다.

```swift
import UIKit

class ViewController: UIViewController, UICollectionViewDataSource, UICollectionViewDelegate {

    // 임의로 지정해준 셀의 갯수
    var numberObCell: Int = 10
    let cellIdentifier: String = "cell"

    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {  // 10개의 셀이 생길것이다.
        return self.numberObCell

    }

    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell: UICollectionViewCell = collectionView.dequeueReusableCell(withReuseIdentifier: self.cellIdentifier, for: indexPath)
        return cell
    }

    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        // 컬렉션뷰의 아이템을 선택하게 되면 셀의 갯수를 1개씩 늘려주는 메서드
        self.numberObCell += 1
        collectionView.reloadData()
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }
}
```

위 코드들을 실행할때 해당 뷰 컨트롤러에 DataSource와 Delegate를 연결해주는 것 잊지말자! 

---
layout: post
title: iOS SCPageControl 사용해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## SCPageControl 사용해보기

이번엔 페이지 컨트롤의 라이브러리인 SCPageControl를 사용하는 방법에 대해 정리해보겠습니당

방법은 엄청 간단합니다. 

```swift 
class HomeTableViewCell: UITableViewCell {
    @IBOutlet weak var collectionView: UICollectionView!
    @IBOutlet weak var sc: SCPageControlView!

    override func awakeFromNib() {
        super.awakeFromNib()
        // 페이지컨트롤에 필요한 페이지 수/페이지가 시작할 위치/현재 보여지는 페이지일때 페이지컨트롤의 색/기본 색 지정 
        sc.set_view(2, current: 0, current_color: UIColor(named: "7A7BDA")!, disable_color: nil)
        // 페이지 컨트롤 스타일 지정 
        sc.scp_style = .SCJAFlatBar
    }
}

extension HomeTableViewCell: UIScrollViewDelegate {
    func scrollViewDidScroll(_ scrollView: UIScrollView) {
        sc.scroll_did(scrollView)
    }
}
```

이때 여기서 가장 중요한 포인트는 코드에 있지않습니다. 

일반적으로 생각해보면 페이지컨트롤이 컬렉션뷰 안에 존재할것같다고 생각할수도 있습니다. <br>
그렇지만 그렇지 않습니다. 컬렉션뷰 바깥에 같은 동일선상에 존재하고 있어야 합니다. 

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/36.png" alt="" width="80%">
</figure>
</center>

이렇게 말이죠!
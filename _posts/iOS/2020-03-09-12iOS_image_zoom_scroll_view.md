---
layout: post
title: Scroll View를 통해 이미지 확대해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Scroll View를 통해 이미지 확대해보기

우선 ImageZoomViewController 파일 하나를 생성해준다.

```swift
import UIKit
import Photos

class ImageZoomViewController: UIViewController, UIScrollViewDelegate {

    // 전화면에서 받아올 asset 프로퍼티
    var asset: PHAsset!
    // imageManager를 통해 이미지 요청
    let imageManager: PHCachingImageManager = PHCachingImageManager()

    @IBOutlet weak var imageView: UIImageView!

    // scrollView delegate 메서드
    func viewForZooming(in scrollView: UIScrollView) -> UIView? {
        // scrollview가 줌을 해줄 대상이 누구? = imageView
        return self.imageView
    }
    override func viewDidLoad() {
        super.viewDidLoad()
        // 메모리에 화면이 로드가 되면 호출될 이미지의 사이즈 등..
        imageManager.requestImage(for: asset, targetSize: CGSize(width: asset.pixelWidth, height: asset.pixelHeight), contentMode: .aspectFill, options: nil, resultHandler: { image, _ in self.imageView.image = image})

        // Do any additional setup after loading the view.
    }
}
```

스토리보드로 넘어서 새로운 뷰컨트롤러를 만들어주고 이전에 만들어놨던 뷰컨트롤러는 네비게이션 컨트롤러 임베드를 해준다.

<center>
<figure>
<img src="/assets/post-img/iOS/81.png" alt="" width="80%">
</figure>
</center>

그리고 스크롤뷰의 delegate는 ImageZoomViewController가 되고 ImageZoomViewController의 imageView는 스토리보드에 생성해놓은 imageView로 연결해준다.

<center>
<figure>
<img src="/assets/post-img/iOS/82.png" alt="" width="80%">
</figure>
</center>
<center>
<figure>
<img src="/assets/post-img/iOS/83.png" alt="" width="80%">
</figure>
</center>

그리고 스크롤뷰와 이미지뷰의 셋팅을 위와같이 설정해준다.

<center>
<figure>
<img src="/assets/post-img/iOS/84.png" alt="" width="80%">
</figure>
</center>
<center>
<figure>
<img src="/assets/post-img/iOS/85.png" alt="" width="80%">
</figure>
</center>

그리고 ViewController로 넘어와 네비게이션 컨트롤러를 통해 전달해줄 데이터 세그를 작성해준다.

```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let nextViewController: ImageZoomViewController = segue.destination as? ImageZoomViewController else { return }
    guard let cell: UITableViewCell = sender as? UITableViewCell else { return }
    guard let index: IndexPath = self.tableView.indexPath(for: cell) else { return }
    nextViewController.asset = self.fetchResult[index.row]
}
```

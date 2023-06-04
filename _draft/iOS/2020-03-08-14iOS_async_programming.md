---
layout: post
title: OperationQueue를 활용하여 비동기 프로그래밍 해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## OperationQueue를 활용하여 비동기 프로그래밍 해보기

이미지를 가져오는 작업을 할때 https 주소를 가진 이미지라면 상관없지만 보안을 중요시하는 iOS에 있어서 만약 https가 아닌 다른 주소의 이미지를 가져오고자할때는 info.plist에 들어와서 별도의 작업을 진행해주어야 한다.

**App Transport Security Settings**

<center>
<figure>
<img src="/assets/post-img/iOS/77.png" alt="" width="80%">
</figure>
</center>
<center>
<figure>
<img src="/assets/post-img/iOS/77.png" alt="" width="80%">
</figure>
</center>

그리고 vc에 돌아와 아래와 같이 코드 진행을 해주도록 한다.

```swift
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var imageView: UIImageView!
    @IBAction func touchUpDownloadBtn(_ sender: UIButton){
        // 이미지 다운로드 후 이미지 뷰에 셋팅
        let imageURL: URL = URL(string: "https://upload.wikimedia.org/wikipedia/commons/3/3d/LARGE_elevation.jpg")!
        // url에서 받아오는 이미지 데이터
        // 이 메서드는 동기메서드로 작업이 끝나기 전까지는 다음줄로 넘어가지를 못한다
        let imageData: Data = try! Data.init(contentsOf: imageURL)
        let image: UIImage = UIImage(data: imageData)!
        self.imageView.image = image
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }
}
```

위와 같이 코드를 진행하게 되면 문제가 발생한것처럼 보이는 현상이 드러날 것이다.

Download라는 버튼을 누르고나면 한동암 freezing 현상이 일어나게 되고 사진이 다운로드되어 이미지뷰에 보여지게 되면 다시 버튼이 활성화 될 것이다. 이는 현재 위 코드는 메인쓰레드에서 동작을 하게 된다. 그렇기 때문에 하나의 동작이 이루어지게 되면 해당 작업이 끝나기 전까지는 나머지 작업을 할 수 없게 된다.

이를 OperationQueue를 통해 메인쓰레드가 아닌 다른 곳에서 동작할수 있도록 해보자!

```swift
OperationQueue().addOperation {
            // url에서 받아오는 이미지 데이터
            // 이 메서드는 동기메서드로 작업이 끝나기 전까지는 다음줄로 넘어가지를 못한다
            let imageData: Data = try! Data.init(contentsOf: imageURL)
            let image: UIImage = UIImage(data: imageData)!
            self.imageView.image = image
            }
        }
```

고쳤음에도 경고가 나타날 것이다.

<center>
<figure>
<img src="/assets/post-img/iOS/79.png" alt="" width="80%">
</figure>
</center>

이미지는 메인쓰레드에서만 사용해야한다. 즉, UI와 관련된 코드는 메인쓰레드에서만 동작을 해야한다.

```swift
OperationQueue().addOperation {
            // url에서 받아오는 이미지 데이터
            // 이 메서드는 동기메서드로 작업이 끝나기 전까지는 다음줄로 넘어가지를 못한다
            // 따라서 이미지를 가져오는 작업은 새 쓰레드를 만들어 백그라운드 작업을 하고 이 작업이 끝나면
            let imageData: Data = try! Data.init(contentsOf: imageURL)
            let image: UIImage = UIImage(data: imageData)!

            OperationQueue.main.addOperation {
                // 메인쓰레드에 가져와서 아래 코드를 실행해달라는 것
                self.imageView.image = image
            }
        }
```

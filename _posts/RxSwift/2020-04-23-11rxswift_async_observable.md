---
layout: post
title: RxSwift 비동기 작업과 Observable
category: RxSwift
tags: [RxSwift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

공부에 앞서 해당 깃헙을 클론해 왔습니다 > [gitHub 바로가기](https://github.com/iamchiwon/RxSwift_In_4_Hours)


```swift
// MARK: - IBAction

@IBAction func onLoadSync(_ sender: Any) {
    guard let url = URL(string: loadingImageUrl) else { return }  // url과
    guard let data = try? Data(contentsOf: url) else { return }  // data를 가져와

    let image = UIImage(data: data)  // UIImage를 만들어서 셋팅해준다
    imageView.image = image
}

// 위코드를 그대로 가져와 비동기로 만들고 싶다면 DispatchQueue를 사용해주면 된다
@IBAction func onLoadAsync(_ sender: Any) {
    DispatchQueue.global().async {
      guard let url = URL(string: loadingImageUrl) else { return }
      guard let data = try? Data(contentsOf: url) else { return }

      let image = UIImage(data: data)

      DispatchQueue.main.async {
        self.imageView.image = image
      }
  }
}
```

DispatchQueue에는 sync와 async가 있고 concurrencyQueue와 serialQueue가 있다.

DispatchQueue의 global = concurrencyQueue
concurrency는 동시에 라는 의미

- concurrencyQueue sync
- concurrencyQueue async
- serialQueue sync
- serialQueue async



```swift
// MARK: - PromiseKit

func promiseLoadImage(from imageUrl: String) -> Promise<UIImage?> {
    return Promise<UIImage?>() { seal in
        asyncLoadImage(from: imageUrl) { image in
            seal.fulfill(image)
        }
    }
}
```

> RxSwift는 결국 비동기 작업을 간결하게 하기 위해 쓰는 것


promise kit에서는 promise return 해서 new > ceal > fullfill<br>
observable 리턴하기 위해서 observable create > ceal > on next로 데이터를 넘겨줌

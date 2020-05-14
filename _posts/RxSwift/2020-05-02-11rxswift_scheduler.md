---
layout: post
title: Rxswift Scheduler
category: RxSwift
tags: [RxSwift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## observeOn

observeOn이 걸리게 되면 그 아래는 observeOn에서 동작하도록 한 명령 아래 실행이 되도록 한다.

```swift
@IBAction func exMap3() {
    //
    Observable.just("800x600")
        .observeOn(ConcurrentDispatchQueueScheduler(qos: default))  // 동시에 실행하게 하면서 백그라운드에서 실행하도록!
        .map { $0.replacingOccurrences(of: "x", with: "/") }  // "800/600"
        .map { "https://picsum.photos/\($0)/?random" }  // "https://picsum.photos/800/600/?random"
        .map { URL(string: $0) }  // URL?
        .filter { $0 != nil }
        .map { $0! }  // URL!
        .map { try Data(contentsOf: $0) }  // Data
        .map { UIImage(data: $0) }  //UIImage?
        .observeOn(MainScheduler.instance)  // 실행(subscribe)전에 메인쓰레드에서 동작하도록! 아래부터 메인쓰레드에서 동작
        .subscribe(onNext: { image in
            self.imageView.image = image
        })
        .disposed(by: disposeBag)
}
```

그런데 만약 처음 just에서부터 데이터를 받아오게 된다면 어떻게 해야할까?


#### subscribeOn

subscribeOn의 위치는 어디에 있어도 상관없다. 심지어 데이터를 처리하는 곳 이후에 넣어도 된다.<br>
그 이유는 subscribe 될때부터 적용되기 시작하기 때문이다.

subscribe를 쓰는 순간 그때부터 모든 just 즉, 오퍼레이터의 시작이 이루어진다.


```swift
@IBAction func exMap3() {
    //
    Observable.just("800x600")
        .map { $0.replacingOccurrences(of: "x", with: "/") }  // "800/600"
        .map { "https://picsum.photos/\($0)/?random" }  // "https://picsum.photos/800/600/?random"
        .map { URL(string: $0) }  // URL?
        .filter { $0 != nil }
        .map { $0! }  // URL!
        .subscribeOn(Scheduler: ConcurrentDispatchQueueScheduler(qos: default))  // subscribeOn을 사용!
        .map { try Data(contentsOf: $0) }  // Data
        .map { UIImage(data: $0) }  //UIImage?
        .observeOn(MainScheduler.instance)  
        .subscribe(onNext: { image in
            self.imageView.image = image
        })
        .disposed(by: disposeBag)
}
```

위 코드에서 이미지를 처리할때 나타나는 부작용을 처리하기 위해서는 do()를 사용한다.

```swift
@IBAction func exMap3() {
    //
    Observable.just("800x600")
        .map { $0.replacingOccurrences(of: "x", with: "/") }  // "800/600"
        .map { "https://picsum.photos/\($0)/?random" }  // "https://picsum.photos/800/600/?random"
        .map { URL(string: $0) }  // URL?
        .filter { $0 != nil }
        .map { $0! }  // URL!
        .subscribeOn(Scheduler: ConcurrentDispatchQueueScheduler(qos: default))  // subscribeOn을 사용!
        .map { try Data(contentsOf: $0) }  // Data
        .map { UIImage(data: $0) }  //UIImage?
        .observeOn(MainScheduler.instance)  
        .do(onNext: ((UIImage?) throws -> Void)?, afterNext: ((UIImage?) throws -> Void)?, onError: ((Error) throws -> Void)? , afterError: ((Error) throws -> Void)?, onCompleted: (() throws -> Void)?, afterCompleted: (() throws -> Void)?, onSubscribe: (() -> Void)?, onSubscribed: (() -> Void)?, onDispose: (() -> Void)?)
        .subscribe(onNext: { image in
            self.imageView.image = image
        })
        .disposed(by: disposeBag)
}
```

---
layout: post
title: Rxswift Operators(next, error, completed)
category: RxSwift
tags: [RxSwift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## subscribe(next, error, completed)

```swift
@IBAction func exJust1() {
    // just에 무언가를 넣어주면 그 다음 subscribe를 해주면 첫번쨰 인자로 바로 넘어가게 해준다
    Observable.just("Hello World")  // 최종적으로 생성된 스트림을 사용하려고 할때 선언하는 것이 subscribe
        .subscribe() // 최종으로 얻은 데이터를 받아서 사용할 수 있음
}
```

- subscribe(): 그냥 단순히 출력만을 원할 경우. 실행만 시키고 그 내용은 별로 신경쓰지 않아도 될때
- subscribe(on: (Event<String>) -> Void##(Event<String>) -> Void): 가장 기본적인 형태
  - event타입에는 3개지 종류가 있다.

```swift
@IBAction func exJust1() {
    Observable.just("Hello World")
        .subscribe { event in
            switch event {
                case .next(let str):  // 데이터가 전달되는 것
                    break
                case .error(let err):  // 에러가 났을때
                    break
                case .completed:  // 모든 스트림이 완료되었을 때
                    break
              }}
        .disposed(by: disposeBag)        
}
```

다른 오퍼레이터들은 리턴타입이 스트림(=observable이라는 뜻)이지만, subscribe는 disposable이다.

- subscribe(onNext: ((String) -> Void)?, onError: ((Error) -> Void)?, onCompleted: (() -> Void)?, onDisposed: (() -> Void)?)
  - completed도 disposed, error도 disposed!

```swift

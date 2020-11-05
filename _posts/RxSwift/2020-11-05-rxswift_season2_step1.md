---
layout: post
title: RxSwift 곰튀김님 강의 season2, step1 정리 (1)
category: RxSwift
tags: [RxSwift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
본 내용은 공식문서를 참고하여 정리하였습니다.

<hr>

## RxSwift Season2, step1 정리

- **rx의 용도는 비동기적으로 생기는 데이터를 return값으로 전달**
- 그 값을 사용할때는 subscribe를 사용하면 됨
- Observable형태로 감싸서 리턴하면 이것은 나중에 생기는 데이터이고
- 데이터를 사용하기 위해서는 subscribe를 사용하면 되고
- 이때 event가 발생하는데 event의 종류는 총 3가지이다.
- next(데이터를 전달할때), completed(데이터가 완전히 다 전달되었을때), error


### RxSwift의 순환참조 해결 방법

```swift
@IBAction func onLoad() {
  //
  let disposable = downloadJson(MEMBER_LIST_URL)
    .subscribe { [weak self] event in
      switch event {
      case let .next(json):
        self?.editView.text = json

        case .completed:
          break
        case .error:
          break
      }
    }
  // disposable.dispose()
}
```

- 이 경우에서 **순환 참조가 발생**한다.
  - 순환참조가 발생한다는 것은 클로저가 self를 캡쳐하면서 reference count가 증가하기 때문
  - 감소하기 위해서는 클로저가 없어지면 reference count가 감소
  - **없어지는 그 시점은 completed 혹은 error가 실행될 때** > 순환참조문제 해결!


## 비동기로 생기는 데이터를 Observable로 감싸서 리턴하는 방법

```swift
func downloadJson(_ url: String) -> Observable<String?> {
  // 비동기로 생기는 데이터를 Observable로 감싸서 리턴하는 방법
  return Observable.create{ f in
    DispatchQueue.global().async {
      let url = URL(string:url)
      let data = try! Data(contentsOf: url)
      let json = String(data: data, encoding: .utf8)

      DispatchQueue.main.async {
        f.onNext(json)
        f.onCompleted()
      }
    }
    return Disposables.create()
  }
}
```

다시 한번 만들어본다.

```swift
func downloadJson(_ url: String) -> Observable<String?> {
  return Observable.crate { emitter in // Observable을 만들고 여기에 들어가는 인자로 클로저 생성
    emitter.onNext("Hello") // 데이터 전달 && 여러개를 전송할수도 있음
    emitter.onNext("World")
    emitter.onCompleted() // 데이터 전송이 끝났다.

    return Disposables.create()  
  }
}
```

- 이렇게 되면 Hello라는 데이터는 onLoad()의 event로 들어오게 된다.
- Hello가 들어오고 World가 들어오면서 결과적으로는 World만 남아있을것이다.
  - World가 덮어씀


### 쓰레드 관점에서...


```swift
func downloadJson(_ url: String) -> Observable<String?> {
  return Observable.crate { emitter in
    let url = URL(string: url)!
    URLSession.shared.dataTask(with: url) { (data, _, err) in
      guard err == nil else {
        emitter.onError(err!)  // 에러가 발생하면 에러를 보낸다
        return
        }
      if let dat = data, let json = String(data: dat, encoding: .utf8) {
        emitter.onNext(json)  // data가 준비가 되면 onNext
      }
      emitter.onCompleted()  // data가 준비되지 않으면 onCompleted
    }
    task.resume()
    return Disposables.create() {
      task.cancle()
    }
  }
}
```

- URLSession는 메인쓰레드가 아닌 다른 쓰레드에서 처리 되는데
- error, next, completed는 모두 다른 쓰레드에서(URLSession이 처리되는 쓰레드) 처리가 된다.

자 그런데 결국 데이터는 Next로 json이 전달되어지는데, 이 과정 모두 메인쓰레드가 아닌 곳에서 실행하고<br>
이를 받는 아래 코드 또한 메인쓰레드가 아닌 다른곳(URLSession이 동작하는 쓰레드)에서 실행하게 된다.

```swift
downloadJson(MEMBER_LIST_URL)
  .subscribe { event in
    switch event {
      case let .next(json):
        self.editView.text = json // json이 동작하는 이 부분은 ui를 바꾸는 부분! main쓰레드를 사용해야함
        self.setVisibleWithAnimation(self.activityIndicator, false)
      case .completed:
        break
      case .error:
        break
      }
    }
```

따라서 아래 코드처럼 진행해야 한다.

```swift
downloadJson(MEMBER_LIST_URL)
  .subscribe { event in
    switch event {
      case let .next(json):
        DispatchQueue.main.async {
          self.editView.text = json
          self.setVisibleWithAnimation(self.activityIndicator, false)
        }
      case .completed:
        break
      case .error:
        break
      }
    }
```

### Observable의 생명주기

1. Create > Observable이 **create가 되었다고 해서 무조건 데이터가 전달되는 것은 아니다.**
2. Subscribe > **Observable이 동작하는 순간**은 이때다.
3. onNext > 데이터 전달
4. onCompleted / onError
5. Disposed

**한번 subscribe를 통해 동작이 시작되면 completed, error, disposed를 통해 동작이 끝난다.**<br>
그리고 **동작이 끝난 Observable은 재사용이 불가능**하다.

### debug

```swift
downloadJson(MEMBER_LIST_URL)
  .debug() // 이 사이에 들어오는 데이터가 찍힘
  .subscribe { event in
    switch event {
      case let .next(json):
        DispatchQueue.main.async {
          self.editView.text = json
          self.setVisibleWithAnimation(self.activityIndicator, false)
        }
      case .completed:
        break
      case .error:
        break
      }
    }
```


## Observable로 오는 데이터를 받아서 처리하는 방법

```swift
@IBAction func onLoad() {
  editView.text = ""
  setVisibleWithAnimation(activityIndicator, true)

  let observable = downloadJson(MEMBER_LIST_URL)
  let disposable = observable.subscribe { event in
      switch event {
      case let .next(json):
          break
        case .completed:
          break
        case .error:
          break
      }
    }
  // disposable.dispose()
}
```

- 기본적으로 subscribe함으로써 event를 받고
- 그 event의 next, error, completed를 처리할 것인지 하게 됨
- 이때의 순환참조 문제도 completed or error or disposed가 되었을때 해결된다.
  - 그러나 이런 방법을 쓰는것보다 **weak self**를 사용하는것이 가장 안전

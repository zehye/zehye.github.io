---
layout: post
title: DispatchQueue란? +실습까지!
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.     

<hr>

## DispatchQueue

DispatchQueue는 작업항목의 실행을 관리하는 클래스이다. 대기열(Queue)에 추가된 작업항목은 시스템이 관리하는 스레드 풀에서 처리하고 작업을 완료하면 스레드를 알아서 해제합니다. DispatchQueue의 장점은 일반 스레드 코드보다 쉽고 효율적으로 코드를 작성할 수 있다는 점이다. 주로 **iOS에서는 서버에서 데이터를 내려받는다든지 이미지, 동영상 등 멀티미디어 처리와 같이 CPU 사용량이 많은 처리를 별도의 스레드에서 처리한 뒤 메인 스레드로 결과를 전달하여 화면에 표시** 한다. 그리고 DispatchQueue를 생성 시 기본은 **Serial** 로, Concurrent 유형으로 바꾸려면 별도로 명시만 해주면 된다.

### 대기열(Queue) 유형

- Serial: 대기열(Queue)에 등록한 순서대로 작업을 실행
  - 하나의 작업을 실행하고 실행이 끝날 때까지 대기열(Queue)에 있는 다른 작업을 미루고 있다가 이전 작업이 끝나면 실행.
- Concurrent: 실행 중인 작업이 끝나기를 기다리지 않고 대기열(Queue)에 있는 작업을 동시에 별도의 스레드를 사용하여 실행! >  즉, 병렬처리 방식

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/23.png" alt="" width="50%">
</figure>
</center>



#### 주요 프로퍼티

```swift
// main : 애플리케이션의 메인 스레드와 연결된 Serial DispatchQueue를 반환
class var main: DispatchQueue { get }

// label : 대기열(Queue)을 식별하기 위한 문자열 레이블
var label: String { get }​

// qos : DispatchQoS구조체의 타입의 프로퍼티. 대기열 작업을 효율적으로 수행할 수 있도록 여러 우선순위 옵션을 제공
 var qos: DispatchQoS { get }​
```

#### 주요 메서드

```swift
// sync(execute:) : DispatchQueue에서 실행을 위해 클로저를 대기열(Queue)에 추가하고 해당 클로저가 완료될 때까지 대기
func sync(execute block: () -> Void)​

// async(execute:) : DispatchQueue에서 비동기 실행을 위해 클로저를 추가하고 즉시 실행
func async(execute workItem: DispatchWorkItem)​

// asyncAfter(deadline:execute:) : 지정된 시간에 실행하기 위해 클로저를 DispatchQueue에 추가 > 그리고 지정된 시간이 지나면 바로 실행
func asyncAfter(deadline: DispatchTime, execute: DispatchWorkItem)

// global(qos:) : 시스템의 글로벌 대기열(Queue)을 반환
class func global(qos: DispatchQoS.QoSClass = default) -> DispatchQueue
```


## 실습

[이전 실습에서의 문제점](https://www.zehye.kr/ios/2020/04/09/13iOS_urlsession_datatask/)

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/24.png" alt="" width="50%">
</figure>
</center>

해당 링크 맨 아래의 실습을 들어가게 되면 tableView.reload() 메서드가 백그라운드 쓰레드에서 실행이 되면서 작은 문제가 생긴것을 확인할 수 있다. (해당 함수는 만드시 메인 쓰레드에서 동작되어야 한다!)

그래서 이 메서드를 메인 쓰레드에서 동작할 수 있도록 해보자!

```swift
DispatchQueue.main.async {
  self.tableView.reloadData()
}
```

본래 저 메서드가 존재하던 클로저는 백그라운드 쓰레드에서 동작을 해야했다. 그러나 해당 메서드는 반드시 메인 쓰레드에서만 동작해야하기에 해당 코드를 DispatchQueue 메인으로 옮겨주면 문제없이 동작할 것이다.

```swift
let dataTask: URLSessionDataTask = session.dataTask(with: url) { (data: Data?, response: URLResponse?, error: Error?) in
    if let error = error {
        print(error.localizedDescription)
        return
    }

    guard let data = data else {return}
    do {
        let apiResponse: APIResponse = try JSONDecoder().decode(APIResponse.self, from: data)
        self.friends = apiResponse.results

        DispatchQueue.main.async {
            self.tableView.reloadData()
        }

    } catch(let err) {
        print(err.localizedDescription)
    }
}
```

그리고 더 나아가 우리가 이미지를 다운로드 해오고 있는 코드는 아래와 같이 이미지가 동기 메서드이기 때문에 이미지 다운로드가 끝날때까지 다운로드가 멈춰있다. `Data(contentsOf: imageURL)`

```swift
guard let imageData: Data = try? Data(contentsOf: imageURL) else { return cell }
```

따라서 이 문제를 해결하기 위해 이미지 다운로드를 받는 것은 백그라운드에서 실행을 하고 이 이미지를 셋팅하는 것은 메인에서 실행할 수 있도록 해줘야한다!

```swift
// 백그라운드에서 동작하는 큐(기본적으로 동작하는 큐)
DispatchQueue.global().async {
    guard let imageURL: URL = URL(string: friend.picture.thumbnail) else { return cell }
    guard let imageData: Data = try? Data(contentsOf: imageURL) else { return cell }

    // 메인에서 동작하도록 하는 코드
    DispatchQueue.main.async {
        cell.imageView?.image = UIImage(data: imageData)
    }
}
```

그런데 이렇게 해도 문제가 발생한다. 사용자는 우리가 이미지를 다운받는 중에도 계속해서 셀을 움직이는데(화면을 위아래로 움직인다) 이는 셀의 인덱스가 바뀐다는 것으로 사용자가 보는 셀과 이미지가 다운받아지는 셀의 위치가 상이해질 수 있음을 의미한다. 따라 이를 구분하면서 일치하는 상황에서만 이미지를 셋팅해줄 수 있어야 한다.

```swift
DispatchQueue.global().async {
    guard let imageURL: URL = URL(string: friend.picture.thumbnail) else { return }
    guard let imageData: Data = try? Data(contentsOf: imageURL) else { return }

    DispatchQueue.main.async {
        // tableView의 indexPath cell을 통해 index를 만들어주고
        if let index: IndexPath = tableView.indexPath(for: cell) {
            // 인덱스의 로우가 현재 테이블뷰를 셋팅해줬을때의 indexPath 같다면
            if index.row == indexPath.row {
                // 이때 cell에 이미지를 셋팅해달라!
                cell.imageView?.image = UIImage(data: imageData)
            }
        }
    }
}
```


더 나아가 해당 셀이 재사용되기 전에 셀의 이미지 뷰의 이미지는 비워놔야한다! 나중에 이미지를 가져오기 전에 다른 사람의 이미지는 표현이 안되어있는 상태로 있어야하기 때문이다. 

```swift
cell.imageView?.image = nil
```

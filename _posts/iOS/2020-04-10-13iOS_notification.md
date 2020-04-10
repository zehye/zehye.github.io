---
layout: post
title: Notification Center와 Notification +예제
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.     

<hr>

## Notification

등록된 노티피케이션에 노티피케이션 센터를 통해 정보를 전달하기 위한 구조체  >> 게임을 생각하면 쉬움(크아에서 물풍선에 갇히는 범위)

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/25.png" alt="" width="50%">
</figure>
</center>


### 주요 프로퍼티

```swift
// name : 알림을 식별하는 태그
var name: Notification.Name

// object : 발송자가 옵저버에게 보내려고 하는 객체. 주로 발송자 객체를 전달하는 데 쓰임
var object: Any?

// userInfo : 노티피케이션과 관련된 값 또는 객체의 저장소
var userInfo: [AnyHashable : Any]?​
```

예) 특정 행동으로 인해 작업이 시작되거나 완료되는 시점에 다른 인스턴스로 노티피케이션이 발생 시 필요한 데이터를 같이 넘겨 줄 수 있다. 간단한 예로 네트워킹을 이용하는 애플리케이션이라면 네트워킹이 시작 및 완료되는 시점, 음악 및 동영상 재생 등에도 재생이 끝나는 시점에 관련 정보를 넘겨 줄 수 있다.


## NotificationCenter

등록된 옵저버에게 동시에 노티피케이션을 전달하는 클래스이다. NotificationCenter 클래스는 노티피케이션을 발송하면 노티피케이션 센터에서 메세지를 전달한 옵저버의 처리할 때까지 대기한다. 즉, 흐름이 동기적(synchronous)으로 흘러가게 되어 노티피케이션을 비동기적으로 사용하려면 NotificationQueue를 사용하면 된다


### 기본 노티피케이션 센터 얻기

```swift
// default : 애플리케이션의 기본 노티피케이션 센터
class var `default`: NotificationCenter { get }​
```

### 옵저버 추가 및 제거




## 실습

Notification을 보낼 Request.swift 파일을 만들어준다.

```swift
import Foundation

// Notification 이름을 하나 만들어줌
let DidReceiveFriendNotification: Notification.Name = Notification.Name("DidReceiveFriend")

func requestFriend() {
    guard let url: URL = URL(string: "https://randomuser.me/api/?results=20&inc=name,email,picture") else {return}

    let session: URLSession = URLSession(configuration: .default)
    let dataTask: URLSessionDataTask = session.dataTask(with: url) { (data: Data?, response: URLResponse?, error: Error?) in

        if let error = error {
            print(error.localizedDescription)
            return
        }

        guard let data = data else {return}
        do {
            let apiResponse: APIResponse = try JSONDecoder().decode(APIResponse.self, from: data)

            // userInfo 딕셔너리에 friends와 받아온 results값을 실어서 보내도록 한다
            NotificationCenter.default.post(name: DidReceiveFriendNotification, object: nil, userInfo: ["friends": apiResponse.results])

        } catch(let err) {
            print(err.localizedDescription)
        }
    }
    dataTask.resume()
}
```

그리고 notification center를 통해 보낸 request를 받아주는 함수를 viewController에 작성해준다.

```swift
// request.swift에서 center통해 보냈으니 받아주는 함수
@objc func didReceiveFriendNotification(_ noti: Notification) {
    // friends를 vc의 프로퍼티에 셋팅해준다음 테이블뷰 리로드 해줘야 함
    // noti.UserInfo에 실어서 보냈던 정보를 friends라고 해서 해당 정보를 받아오도록 한다
    // UserInfo가 있는지 없는지 모르니까 ?로 까본다
    guard let friends: [Friend] = noti.userInfo?["friends"] as? [Friend] else { return }

    // 위에서 만들어준 friends를 넣어주면 됨
    self.friends = friends

    // notification을 발송하는 쓰레드와 받는 쪽도 같은 쓰레드에서 동작하도록 해야한다.
    // 따라서 메인쓰레드에서 동작해야하는 쓰레드는 반드시 해당 코드로 작성해주어야 한다.
    // 혹은 notification 센터를 호출할때 메인쓰레드에서 호출하도록 하면 되기도 한다.
    DispatchQueue.main.async {
        self.tableView.reloadData()
    }
}

override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view.

    // Notification을 받을 것이라고 센터에 알려줌
    // 내가 받고(self) 어떤 notification을 함수로 받을 것인지, 어떤 이름으로 가져온 것을 받을 것인지를 등록
    // notification 센터에 등록을 하고 notification이 발생하면 해당 메서드를 통해 알려주면 해당 메서드가 실행될 것이라고 알려줌
    NotificationCenter.default.addObserver(self, selector: #selector(self.didReceiveFriendNotification(_:)), name: DidReceiveFriendNotification, object: nil)
}

override func viewDidAppear(_ animated: Bool) {
    super.viewDidAppear(animated)
    requestFriend()
}
```

notification은 잘 사용하면 굉장히 좋은 무기지만 잘못 사용하게 되면 굉장히 헷갈리게 된다.

따라서 해당 관련된 것은 상수로 잘 정리해놓고 key값도 정리하는것이 좋으며, 위 실습에서의 코드에서만 보아도 코드가 굉장히 분산히 되어보이는데, 그렇기 때문에 코드가 간단한 경우에는 안쓰는 것이 효율적이지만 한번에 여러 인스턴스에게 notification을 전달해야한다면 이를 사용하는 것이 좋다.

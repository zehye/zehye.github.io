---
layout: post
title: URLSession과 URLSessionDataTask
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.     

<hr>

## URLSession

## URLSessionDataTask

URLSession은 HTTP/HTTPS를 통해 콘텐츠(데이터)를 주고받는 API를 제공하는 클래스이다. 이 API는 인증 지원을 위한 많은 델리게이트 메서드를 제공하며, 애플리케이션이 실행 중이지 않거나 일시 중단된 동안 백그라운드 작업을 통해 콘텐츠를 다운로드하는 것을 수행하기도 한다. URLSession API를 사용하기 위해 애플리케이션은 세션을 생성한다. 해당 세션은 관련된 데이터 전송작업 그룹을 조정한다. 예를 들면 웹 브라우저를 사용 중인 경우 탭 당 하나의 세션을 만들 수 있다. 각 세션 내에서 애플리케이션은 작업을 추가하고, 각 작업은 특정 URL에 대한 요청을 나타낸다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/19.png" alt="" width="80%">
</figure>
</center>


- Request: 서버로 요청을 보낼 때 어떤 (HTTP)메서드를 사용할 것인지, 캐싱 정책은 어떻게 할 것인지 등을 설정
- Response: URL 요청의 응답을 나타내는 객체


### 세션의 유형

URLSession API는 세가지 유형의 세션을 제공한다. 이 타입은 URLSession 객체가 소유한 configuration 프로퍼티 객체에 의해 결정된다.

- 기본 세션 (Default Session): URL 다운로드를 위한 다른 파운데이션 메서드와 유사하게 동작. 디스크에 저장하는 방식이다
- 임시 세션 (Ephemeral Session): 기본 세션과 유사하지만, 디스크에 어떤 데이터도 저장하지 않고, 메모리에 올려 세션과 연결한. 따라서 애플리케이션이 세션을 만료시키면 세션과 관련한 데이터가 사라진다.
- 백그라운드 세션 (Background Session): 별도의 프로세스가 모든 데이터 전송을 처리한다는 점을 제외하고는 기본 세션과 유사


### 세션 만들기

```swift
// init(configuration:) : 지정된 세션 구성으로 세션을 만든다
 init(configuration: URLSessionConfiguration)

// shared : 싱글턴 세션 객체를 반환
 class var shared: URLSession { get }​
```

### 세션 구성

```swift
// configuration : 세션에 대한 구성 객체
@NSCopying var configuration: URLSessionConfiguration { get }

// delegate : 세션의 델리게이트
 var delegate: URLSessionDelegate? { get }
```


## Task

URLSessionTask는 세션 작업 하나를 나타내는 추상 클래스로 하나의 세션 내에서 URLSession 클래스는 세 가지 작업 유형, 즉 데이터 작업(Data Task), 업로드 작업(Upload Task), 다운로드 작업(Download Task)을 지원한다.


## URLSessionDataTask

HTTP의 각종 메서드를 이용해 서버로부터 응답 데이터를 받아서 Data 객체를 가져오는 작업을 수행한다.


### URLsessionUploadTask

애플리케이션에서 웹 서버로 Data 객체 또는 파일 데이터를 업로드하는 작업을 수행합니다. 주로 HTTP의 POST 혹은 PUT 메서드를 이용합니다.


### URLSessionDownloadTask

서버로부터 데이터를 다운로드 받아서 파일의 형태로 저장하는 작업을 수행한다. 애플리케이션의 상태가 대기 중이거나 실행 중이 아니라면 백그라운드 상태에서도 다운로드가 가능하다.
데이터 작업은 서버로부터 어떤 응답이라도 Data 객체의 형태로 전달받을 때 사용하며, 업로드 작업 및 다운로드 작업은 단순한 바이너리 파일의 전달에 목적을 둔다고 볼 수 있다.
JSON, XML, HTML 데이터 등 단순한 데이터의 전송에는 주로 데이터 작업을 사용하며, 용량이 큰 파일의 경우 애플리케이션이 백그라운드 상태인 경우에도 전달할 수 있도록 업로드(다운로드) 작업을 주로 사용한다.



#### 세션에 Data Task 추가하기

```swift
dataTask(with:) : URL에 데이터를 요청하는 데이터 작업 객체를 만듭니다.
func dataTask(with url: URL) -> URLSessionDataTask​
dataTask(with:) : URLRequest 객체를 기반으로 URL에 데이터를 요청하는 데이터 작업 객체를 만듭니다.
func dataTask(with request: URLRequest) -> URLSessionDataTask
dataTask(with:completionHandler:) : URL에 데이터를 요청하고 요청에 대한 응답을 처리할 완료 핸들러(Completion Handler)를 갖는 데이터 작업 객체를 만듭니다.
func dataTask(with url: URL, completionHandler: @escaping (Data?, URLResponse?, Error?) -> Void) -> URLSessionDataTask
dataTask(with:completionHandler:) : URLRequest 객체를 기반으로 URL에 데이터를 요청하고 요청에 대한 응답을 처리할 완료 핸들러(Completion Handler)를 갖는 데이터 작업 객체를 만듭니다.
func dataTask(with request: URLRequest, completionHandler: @escaping (Data?, URLResponse?, Error?) -> Void) -> URLSessionDataTask
```


#### 세션에  Download Task 추가하기

```swift
// downloadTask(with:) : URL에 요청한 데이터를 다운로드 받아서 파일에 저장하는 다운로드 작업을 만든다
func downloadTask(with url: URL) -> URLSessionDownloadTask​

// downloadTask(with:completionHandler:) : URL에 요청한 데이터를 다운로드 받아서 파일에 저장하고 저장 완료 후 완료 핸들러를 호출하는 다운로드 작업을 만든다
func downloadTask(with url: URL, completionHandler: @escaping (URL?, URLResponse?, Error?) -> Void) -> URLSessionDownloadTask

// downloadTask(with:) : URLRequest 객체를 기반으로 URL에 요청한 데이터를 다운로드 받아서 파일로 저장하는 다운로드 작업을 만든다
func downloadTask(with request: URLRequest) -> URLSessionDownloadTask

// downloadTask(with:completionHandler:): URLRequest 객체를 기반으로 URL에 요청한 데이터를 다운로드 받아서 파일로 저장하고 완료 후 완료 핸들러를 호출하는 다운로드 작업을 만든다
func downloadTask(with request: URLRequest, completionHandler: @escaping (URL?, URLResponse?, Error?) -> Void) -> URLSessionDownloadTask
```

#### 세션에 Upload Task 추가하기

```swift
// uploadTask(with:from:): URLRequest 객체를 기반으로 URL에 데이터를 업로드하는 작업을 만든다
func uploadTask(with request: URLRequest, from bodyData: Data) -> URLSessionUploadTask​

// uploadTask(with:from:completionHandler:) : URLRequest 객체를 기반으로 URL에 데이터를 업로드하고 업로드 완료 후 완료 핸들러를 호출하는 작업을 만든다
func uploadTask(with request: URLRequest, from bodyData: Data?,
completionHandler: @escaping (Data?, URLResponse?, Error?) -> Void) -> URLSessionUploadTask

// uploadTask(with:fromFile:) : URLRequest 객체를 기반으로 URL에 파일을 업로드하는 업로드 작업을 만든다
func uploadTask(with request: URLRequest, fromFile fileURL: URL) -> URLSessionUploadTask

// uploadTask(with:fromFile:completionHandler:) : URLRequest 객체를 기반으로 URL에 파일을 업로드하고 업로드 완료 후 완료 핸들러를 호출하는 업로드 작업을 만든다
func uploadTask(with request: URLRequest, fromFile fileURL: URL,
completionHandler: @escaping (Data?, URLResponse?, Error?) -> Void) -> URLSessionUploadTask
```

#### 작업(태스크) 상태 제어

```swift
// cancel(): 작업을 취소
func cancel()

// resume(): 일시중단된 경우 작업을 다시 시작
func resume()​

// suspend() : 작업을 일시적으로 중단
func suspend()​

// state : 작업의 상태를 나타냄
var state: URLSessionTask.State { get }

// priority : 작업처리 우선순위입니다. 0.0부터 1.0 사이
var priority: Float { get set }
```


## 실습

테이블 뷰에 특정 url에서 가져올 정보를 테이블 뷰에 추가하도록 하겠다.

### main.storyboard


<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/19.png" alt="" width="80%">
</figure>
</center>


### Model.swift

받아올 json 형식에 맞게 Model.swift를 작성해준다.

```swift
import Foundation

struct APIResponse: Codable {
    // results 키를 가지는 오브젝트를 생성
    let results: [Friend]
}

// json 구조를 가지고 swift 타입으로 만든 것
struct Friend: Codable {
    struct Name: Codable {
        let title: String
        let first: String
        let last: String

        var full: String {
            return self.title.capitalized + ". " + self.first.capitalized + " " + self.last.capitalized
        }
    }

    struct Picture: Codable {
        let large: String
        let medium: String
        let thumbnail: String
    }

    let name: Name
    let email: String
    let picture: Picture
}
```


### viewController.swift

```swift
import UIKit

class ViewController: UIViewController, UITableViewDataSource {

    @IBOutlet weak var tableView: UITableView!
    let cellIdentifier: String = "friendCell"

    // 받아온 친구정보를 friends로 받아줌
    var friends: [Friend] = []

    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

        // cell을 dequeue해주고
        let cell: UITableViewCell = tableView.dequeueReusableCell(withIdentifier: self.cellIdentifier, for: indexPath)
        // 친구의 정보를 불러오고
        let friend: Friend = self.friends[indexPath.row]

        // 친구의 이름과 이메일을 레이블에 넣어줌
        cell.textLabel?.text = friend.name.full
        cell.detailTextLabel?.text = friend.email

        // 이미지 url로 이미지뷰에 이미지 셋팅을 해준다
        guard let imageURL: URL = URL(string: friend.picture.thumbnail) else { return cell }
        guard let imageData: Data = try? Data(contentsOf: imageURL) else { return cell }

        cell.imageView?.image = UIImage(data: imageData)

        return cell
    }

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        // 테이블 뷰의 로우의 갯수
        return friends.count
    }
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }

    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)

        // 해당 url로 요청할거임
        guard let url: URL = URL(string: "https://randomuser.me/api/?results=20&inc=name,email,picture") else {return}

        // session을 만들어주고
        let session: URLSession = URLSession(configuration: .default)

        // dataTask를 만들어주는데, 이를 만들때 url로 요청할 것이며
        // 해당 클로저는 요청에 대한 서버의 응답이 왔을 때 호출될 클로저를 의미한다
        // 서버에서 보내준 데이터와 응답 그리고 에러가 발생하면 오류가 들어오게 된다.
        let dataTask: URLSessionDataTask = session.dataTask(with: url) { (data: Data?, response: URLResponse?, error: Error?) in

            // 오류에 대한 응답
            if let error = error {
                print(error.localizedDescription)
                return
            }

            guard let data = data else {return}
            do {
                // 받아온 데이터를 가지고 json decorder를 이용해 apiresponse 형식으로 디코더 해본다
                // response의 결과가 들어와있다면 friends에 넣어주고 테이블뷰를 reload해준다
                let apiResponse: APIResponse = try JSONDecoder().decode(APIResponse.self, from: data)
                self.friends = apiResponse.results

                // tableView.reload 라는 메서드는 메인쓰레드에서 호출해줘야하지만
                // 해당 클로저는 메인쓰레드에서 동작하지 않는다.
                // 네트워크 끝내고 받아온 데이터를 처리할때는 많은 양의 데이터가 올 수 있기에 메인쓰레드에서 처리하지않아 백그라운드에서 해결하게 되기에 이를 처리해줘야한다.
                self.tableView.reloadData()
            } catch(let err) {
                print(err.localizedDescription)
            }
        }

        // 이때 dataTask를 실행하고 서버에 요청하게 된다.
        // 해당 클로저는 당장 실행할 코드가 아니라 요청에 대한 응답이 왔을때 실행한다.
        // 실질적으로 실행하는 코드는 세션만들고 데이타테스크 만들어서 실행하는것이 해당 함수에서 동작하는 코드이다.
        dataTask.resume()
    }
}
```
시뮬레이터를 실행해보면 테이블뷰에 데이터가 바로바로 올라오지 못할것이다. 이는 위에서 말했듯 `tableView.reload()`가 메인쓰레드에서 동작해야하지만 현재 백그라운드 쓰레드에서 동작하고 있기 때문이다.
뿐만 아니라 셀의 이미지를 받아오는 이미지가 동기코드로 작성되었기 때문에 이미지가 로드될때마다(셀을 아래로 내릴때마다) 이미지가 끊겨 생기는 것을 확인할 수 있다 > 즉, 다운로드 받는 시간마다 계속 끊기게 된다. >> **dispatchQueue** 로 해결해야한다.

---
layout: post
title: Alamofire란 무엇이고 어떻게 사용하는가?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.   
[참고한 블로그](https://developer-fury.tistory.com/6)

<hr>


## 간단하게...

iOS HTTP통신을 할 때 필수 라이브러리인 Alamofire에 대해서 아주 간단히 정리해보겠다!

Alamofire는 iOS, macOS를 위한 스위프트 기반 HTTP 네트워킹 라이브러리로 Apple의 Foundation networking 기반으로 인터페이스를 제공하여 일반적인 네트워킹 작업을 단순화한다. Alamofire는 함께 사용가능한(chainable) request/response 매소드들, JSON 파라미터, 응답 직렬화(response serialization), 인증(authentication) 그리고 많은 다른 기능을 제공한다.

- 연결가능한(Chainable) Request/Response 메서드
- URL / JSON / plist 파라미터 인코딩
- 파일 / 데이타 / 스트림 / 멀티파트 폼 데이타 업로드
- Request 또는 Resume 데이터를 활용한 다운로드
- NSURLCredential을 통한 인증(Authentication)
- HTTP 리스폰스 검증(Validation)
- TLS 인증서와 공개 키 Pinning
- 진행 상태 클로저와 NSProgress
- cURL 디버깅 출력
- 광범위한 단위 테스트 보장
- [완벽한 문서화](http://cocoadocs.org/docsets/Alamofire/)

즉, 정리해보자면 아래와 같다.

- Alamore란 iOS, macOS를 위한 Swift 기반의 HTTP 네트워킹 라이브러리
- Alamofire는  URLSession 기반이며 URLSession은 네트워킹 호출에서 모호한 부분이 많은데 Alamofire를 사용한다면 데이터를 접근하기 위한 노력을 줄일 수 있으며 코드를 더 깔끔하고 가독성 있게 쓰는 것이 가능해짐

> Alamofire는 HTTP 네트워킹을 하는데 자주 사용하게 되는 코드나 함수를 더 쉽게 사용할 수 있도록 모아놓은 것

iOS에서 기본적으로 제공하고 있는 HTTP통신 방법은 여러가지가 있지만 URLSession을 이용한 방법이 있다. 모두가 같은 방법으로 사용하진 않겠지만 기본적으로 이러한 형태로 요청이 가능하다.


```swift
var request = URLRequest(url: URL(string: "https://api.github.com/users")!)
request.httpMethod = "GET"
URLSession.shared.dataTask(with: request) { (data, response, error) in    
}
```

만약 여기서 Get요청이 POST요청으로 바뀐다고 한다면 아래와 같  파라미터들을 넘길 수 있습니다.

```swift
var request = URLRequest(url: URL(string: "https://api.github.com/users")!)
request.httpMethod = "POST"
let params = ["id":id, "password":password] as Dictionary

do{
  try request.httpBody = JSONSerialization.data(withJSONObject: params, options: [])
} catch {
  return
}

URLSession.shared.dataTask(with: request) { (data, response, error) in }
```

만약에 여기서 Header를 추가하게 된다면 아래를 추가해줘야한다.

```swift
request.addValue("application/json", forHTTPHeaderField: "Content-Type")
request.addValue("application/json", forHTTPHeaderField: "Accept")
```

즉 이렇게 성공, 실패를 또 나눠야한다.

```swift
var request = URLRequest(url: URL(string: "https://api.github.com/users")!)
request.httpMethod = "POST"
let params = ["id":id, "password":password] as Dictionary

do {
  try request.httpBody = JSONSerialization.data(withJSONObject: params, options: [])
} catch {
  return
}

request.addValue("application/json", forHTTPHeaderField: "Content-Type")
request.addValue("application/json", forHTTPHeaderField: "Accept")

URLSession.shared.dataTask(with: request, completionHandler: {(data, response, error) -> Void in
  if let response = response as? HTTPURLResponse, 200...299 ~= response.statusCode {
    //SUCCESS
  }else{
    //Failure
    }
})
```


이런식의 코드는 가독성은 물론이며, 조금의 설정만 바꾸면 많은 것이 변경되어야 해서 여러모로 불편한점이 있다. 그래서 이러한 불편한 것들을 개선시키는 라이브러리인 Alamofire를 사용해보자!


## Alamofire는 Swift로 작성된 HTTP 네트워킹 라이브러리

설치는 CocoaPod으로 진행할 수 있다.


```vim
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '10.0'
use_frameworks!

target '<Your Target Name>' do
    pod 'Alamofire', '~> 4.4'
end
```

이제 설치까지 완료 하였으니 한번 똑같은 URL로 요청해본다!

```swift
Alamofire.request("https://api.github.com/users", method: .get, parameters: [:], encoding: URLEncoding.default, headers: ["Content-Type":"application/json", "Accept":"application/json"])
            .validate(statusCode: 200..<300)
            .responseJSON { (response) in
            if let JSON = response.result.value
            {
                print(JSON)
            }
        }
```
끝이다!

일단 request메소드의 변수들을 보자 > request메소드를 Command + Mouse Left를 클릭해 정보를 보면

request( <br>
  _ url: URLConvertible, <br>
  method: HTTPMethod = .get, <br>
  parameters: Parameters? = nil, <br>
  encoding: ParameterEncoding = URLEncoding.default, <br>
  headers: HTTPHeaders? = nil)


- url: 요청할 URL
- method: 요청 형식 (get, post, put, delete..등)
- parameters: 요청 시 같이 보낼 파라미터
- encoding: encoding
- header: [String:String]형태로 보낼 수 있음


위에서 일일이 추가 하던 것과는 다르게 확연히 다른 차이를 볼 수 있으며, 훨씬 간결해서 보기도 편하다. 뿐만 아니라, request가 성공인지 실패인지를 필터를 하는 validate(statusCode: 200..<300)로 200~299사이의 statusCode결과만 받아올 수 있는 간편한 기능도 지원한다.

저렇게 요청을 하면 아래처럼 쉽게 요청을 할 수 있다.

```json
{
        "avatar_url" = "https://avatars3.githubusercontent.com/u/1?v=3";
        "events_url" = "https://api.github.com/users/mojombo/events{/privacy}";
        "followers_url" = "https://api.github.com/users/mojombo/followers";
        "following_url" = "https://api.github.com/users/mojombo/following{/other_user}";
        "gists_url" = "https://api.github.com/users/mojombo/gists{/gist_id}";
        "gravatar_id" = "";
        "html_url" = "https://github.com/mojombo";
        id = 1;
        login = mojombo;
        "organizations_url" = "https://api.github.com/users/mojombo/orgs";
        "received_events_url" = "https://api.github.com/users/mojombo/received_events";
        "repos_url" = "https://api.github.com/users/mojombo/repos";
        "site_admin" = 0;
        "starred_url" = "https://api.github.com/users/mojombo/starred{/owner}{/repo}";
        "subscriptions_url" = "https://api.github.com/users/mojombo/subscriptions";
        type = User;
        url = "https://api.github.com/users/mojombo";
    },

        {
        "avatar_url" = "https://avatars3.githubusercontent.com/u/2?v=3";
        "events_url" = "https://api.github.com/users/defunkt/events{/privacy}";
        "followers_url" = "https://api.github.com/users/defunkt/followers";
        "following_url" = "https://api.github.com/users/defunkt/following{/other_user}";
        "gists_url" = "https://api.github.com/users/defunkt/gists{/gist_id}";
        "gravatar_id" = "";
        "html_url" = "https://github.com/defunkt";
        id = 2;
        login = defunkt;
        "organizations_url" = "https://api.github.com/users/defunkt/orgs";
        "received_events_url" = "https://api.github.com/users/defunkt/received_events";
        "repos_url" = "https://api.github.com/users/defunkt/repos";
        "site_admin" = 1;
        "starred_url" = "https://api.github.com/users/defunkt/starred{/owner}{/repo}";
        "subscriptions_url" = "https://api.github.com/users/defunkt/subscriptions";
        type = User;
        url = "https://api.github.com/users/defunkt";
    }
```

### 구체적으로 파보기

### Request 만들기

```swift
import Alamofire

Alamofire.request(.GET, "https://httpbin.org/get")

Alamofire.request(.GET, "https://httpbin.org/get", parameters: ["foo": "bar"])
         .responseJSON { response in
             print(response.request)  // original URL request
             print(response.response) // URL response
             print(response.data)     // server data
             print(response.result)   // result of response serialization

             if let JSON = response.result.value {
                 print("JSON: \(JSON)")
             }
         }
```

Alamofire은 비동기(asynchronously)로 네트워크 연동을 하기 때문에 서버로부터 응답을 받을 때까지 기다리지 않고 콜백을 통해서 응답을 처리해준다. 위의 예에서 처럼 요청에 대한 응답은 이를 처리하는 핸들러 안에서만 유효하므로 수신한 응답이나 데이터에 의존적인 동작들은 반드시 해당 핸들러 내에서 완료 해야한다.


### Response 처리

```swift
Alamofire.request(.GET, "https://httpbin.org/get", parameters: ["foo": "bar"])
         .response { request, response, data, error in
             print(request)
             print(response)
             print(data)
             print(error)
          }
```

response 시리얼라이저는 수신한 데이터에 별도의 처리를 하지 않고 URL session 델리게이트로 부터 수신한 모든 정보를 그대로 전달할 뿐이다. Response나 Result 자료형의 장점을 활용할 수 있도록 다른 시리얼라이저를 활용하기를 권장합니다.

Alamofire는 Request뿐만 아니라 Upload, Download등 여러가지를 지원하며 그에 맞는 기능을 최대한 편하게, 간결하게 사용할 수 있도록 제공한다.

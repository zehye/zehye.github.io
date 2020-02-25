---
layout: post
title: JSONEncoder와 JSONDecoder, 코드 실습
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## JSONEncoder / JSONDecoder

스위프트 4 버전 이전에는 JSONSerialization을 사용해 JSON 타입의 데이터를 생성했다. 그러나 스위프트 4 버전부터 JSONEncoder / JSONDecoder가 Codable 프로토콜을 지원하기 때문에 **JSONEncoder / JSONDecoder와 Codable 프로토콜을 이용** 해 손쉽게 JSON 형식으로 인코딩 및 디코딩할 수 있다.

즉, JSONEncoder 및 JSONDecoder를 활용하여 스위프트 타입의 인스턴스를 JSON 데이터로 인코딩, JSON 데이터에서 스위프트 타입의 인스턴스로 디코딩할 수 있다.


### JSONEncoder

Codable 프로토콜을 준수하는 GroceryProduct 구조체의 인스턴스를 JSON 데이터로 인코딩하는 방법

```swift
struct GroceryProduct: Codable {
    var name: String
    var points: Int
    var description: String?
}

let pear = GroceryProduct(name: "Pear", points: 250, description: "A ripe pear.")

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

do {
	let data = try encoder.encode(pear)
	print(String(data: data, encoding: .utf8)!)
} catch {
	print(error)
}

// ----- 출력
 {
   "name" : "Pear",
   "points" : 250,
   "description" : "A ripe pear."
 }

// Tip : encoder.outputFormatting = .prettyPrinted 설정하면 들여쓰기를 통해 가독성이 좋게 출력해줍니다.
```


### JSONDecoder

JSON 데이터를 Codable 프로토콜을 준수하는 GroceryProduct 구조체의 인스턴스로 디코딩하는 방법

```swift
struct GroceryProduct: Codable {
    var name: String
    var points: Int
    var description: String?
}

/// 스위프트 4 버전부터 """를 통해 여러 줄 문자열을 사용할 수 있습니다.
let json = """
{
    "name": "Durian",
    "points": 600,
    "description": "A fruit with a distinctive scent."
}
""".data(using: .utf8)!

let decoder = JSONDecoder()

do {
	let product = try decoder.decode(GroceryProduct.self, from: json)
	print(product.name)
} catch {
	print(error)
}

// ----- 출력
"Durian"
```


## 실제 실습해보기

새 프로젝트를 하나 만들고 기존에 공유받은 json파일을 assets폴더에 import 해준다.

json 예시

```json
{
    "name":"하나",
    "age":22,
    "address_info": {
        "country":"대한민국",
        "city":"울산"
    }
}
```

해당 코드를 처리해줄 swift File하나를 만들어준다.

<center>
<figure>
<img src="/assets/post-img/iOS/64.png" alt="" width="80%">
</figure>
</center>

그리고 해당 폴더에 아래와 같이 작성해준다.

```swift
struct Friend: Codable {

    struct Address: Codable {
        let country: String
        let city: String
    }

    let name: String
    let age: Int
    let addressInfo: Address
}
```

그리고 Main.storyboard에 tableView하나와 tableViewCell 하나를 만들어준다. 해당 테이블뷰에 연결될 outlet을 viewController에 작성해준 뒤 드래그해서 연결해주고, tableViewCell의 style은 **Subtitle** Identifier: cell로 지정해준다.


<center>
<figure>
<img src="/assets/post-img/iOS/65.png" alt="" width="80%">
</figure>
</center>

그리고 viewController로 넘어가 간단하게 코드를 작성해준다.

```swift
import UIKit

class ViewController: UIViewController, UITableViewDataSource {

    @IBOutlet weak var tableView: UITableView!
    let cellIdentifier: String = "cell"
    var friends: [Friend] = []

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return self.friends.count  // friend의 수만큼 보여달라
    }

    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell: UITableViewCell = tableView.dequeueReusableCell(withIdentifier: self.cellIdentifier, for: indexPath)
        let friend: Friend = self.friends[indexPath.row]
//        cell.textLabel?.text = friend.name + "(\(friend.age))"
//        cell.detailTextLabel?.text = friend.address_info.city + ", " + friend.address_info.country
        cell.textLabel?.text = friend.nameAndAge  // 해당 코드를 사용해주기 위해서 Friend.swift 파일을 수정해주어야 한다
        cell.detailTextLabel?.text = friend.fullAddress

        return cell
    }

    override func viewDidLoad() {
        super.viewDidLoad()  // 친구를 가져옴 > 뷰가 로딩된 이후에 해야할일을 적어줌
        // Do any additional setup after loading the view.
        // assets을 통해 Json 데이터를 가져옴
        let jsonDecorder: JSONDecoder = JSONDecoder()
        guard let dataAssets: NSDataAsset = NSDataAsset(name: "friends") else {
            return  //
        }
        // jsonDecorder를 가지고 데이터 에셋의 데이터를 통해 json 데이터를 통해 friends를 불러옴
        do {
            self.friends = try jsonDecorder.decode([Friend].self, from: dataAssets.data)
        } catch {
            print(error.localizedDescription)
        }
        self.tableView.reloadData()
    }
}
```

<center>
<figure>
<img src="/assets/post-img/iOS/66.png" alt="" width="80%">
</figure>
</center>

viewController의 delegate를 연결해주고 위 코드를 사용하기 위한 Friend.swift 파일 수정

```swift
import Foundation

struct Friend: Codable {

    struct Address: Codable {
        let country: String
        let city: String
    }

    let name: String
    let age: Int
    let addressInfo: Address

    var nameAndAge: String {
        return self.name + "(\(self.age))"
    }

    var fullAddress: String {
        return self.addressInfo.city + ", " + self.addressInfo.country
    }

    enum CodingKeys: String, CodingKey {
        case name, age
        case addressInfo = "addess_info"
    }
}
```

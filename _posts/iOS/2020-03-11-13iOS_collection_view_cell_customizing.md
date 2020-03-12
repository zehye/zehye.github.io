---
layout: post
title: 컬렉션 뷰 셀 커스터마이징 해보기(실습)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 컬렉션 뷰 셀 커스터마이징 해보기

해당 셀에 동적으로 데이터를 가져와 셀에 정보를 가져올 수 있도록 해보자.

이를 위해 이전에 실습했던 friends.json파일을 에셋에 추가해주고 Friend.swift를 그대로 가져와 실행하도록 한다.

```swift
import UIKit

class ViewController: UIViewController, UICollectionViewDataSource, UICollectionViewDelegate {

//    var numberObCell: Int = 10
    let cellIdentifier: String = "cell"
    var friends: [Friend] = []
    @IBOutlet weak var collectionView: UICollectionView!

    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
//        return self.numberObCell
        return self.friends.count
    }

//    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
//        let cell: UICollectionViewCell = collectionView.dequeueReusableCell(withReuseIdentifier: self.cellIdentifier, for: indexPath)
//        return cell
//    }

//    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
//        // 컬렉션뷰의 아이템을 선택하게 되면 셀의 갯수를 1개씩 늘려주는 메서드
//        self.numberObCell += 1
//        collectionView.reloadData()
//    }

    // 셀을 가져오는 메서드
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell: FriendsCollectionViewCell = collectionView.dequeueReusableCell(withReuseIdentifier: self.cellIdentifier, for: indexPath) as! FriendsCollectionViewCell // 강제 캐스팅 고치기!

        // Friend 정보를 가져와 셋팅
        let friend: Friend = self.friends[indexPath.item]
        cell.nameAgeLabel.text = friend.nameAndAge
        cell.adressLadel.text = friend.fullAddress

        return cell
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        let jsonDecorder: JSONDecoder = JSONDecoder()
        guard let dataAsset: NSDataAsset = NSDataAsset(name: "friends") else { return }

        do {
            self.friends = try jsonDecorder.decode([Friend].self, from: dataAsset.data)
        } catch {
            print(error.localizedDescription)
        }
        self.collectionView.reloadData()
    }
}
```

#### 참고 - Friend.swift

```swift
import Foundation

//{
//    "name":"하나",
//    "age":22,
//    "address_info": {
//        "country":"대한민국",
//        "city":"울산"
//    }
//}

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

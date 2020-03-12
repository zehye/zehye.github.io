---
layout: post
title: 사진첩에서 사진을 가져오고 삭제해보기(실습)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 사진첩에서 사진 가져오기

```swift

import UIKit
import Photos

class ViewController: UIViewController, UITableViewDataSource {

    // 사진 목록을 테이블뷰에 보여줄 것
    @IBOutlet weak var tableView: UITableView!
    var fetchResult: PHFetchResult<PHAsset>!
    // 가져온 에셋을 가지고 이미지를 로드해옴
    let imageManager: PHCachingImageManager = PHCachingImageManager()
    let cellIdentifier: String = "cell"

    func requestCollection() {
        // 사진찍으면 사진이 저장되는 카메라롤의 컬렉션을 가져옴
        let cameraRoll: PHFetchResult<PHAssetCollection> = PHAssetCollection.fetchAssetCollections(with: .smartAlbum, subtype: .smartAlbumUserLibrary, options: nil)

        guard let cameraRollColletction = cameraRoll.firstObject else {
            return
        }
        let fetchOptions = PHFetchOptions()
        // 최신순으로 사진을 sort
        fetchOptions.sortDescriptors = [NSSortDescriptor(key: "creationDate", ascending: false)]
        // 그 결과를 fetchResult라는 프로퍼티로 가져옴
        self.fetchResult = PHAsset.fetchAssets(in: cameraRollColletction, options: fetchOptions)
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        let photoAuthorizationStatus = PHPhotoLibrary.authorizationStatus()

        switch photoAuthorizationStatus {
        case .authorized:
            print("접근 허가됨")
            self.requestCollection()
            self.tableView.reloadData()
        case .denied:
            print("접근 불허")
        case .notDetermined:
            print("아직 응답하지 않음")
            PHPhotoLibrary.requestAuthorization({ (status) in
                switch status {
                case .authorized:
                    print("사용자가 허용함")
                    self.requestCollection()
                    self.tableView.reloadData()
                case .denied:
                    print("사용자가 불허함")
                default: break
                }
            })
        case .restricted:
            print("접근 제한")
        }
    }
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return self.fetchResult?.count ?? 0
    }

    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        // cell에 이미지를 하나씩 넣어줌
        let cell: UITableViewCell = tableView.dequeueReusableCell(withIdentifier: self.cellIdentifier, for: indexPath)
        // asset은 fetchresult에서 index에 해당
        let asset: PHAsset = fetchResult.object(at: indexPath.row)
        // imageManager를 통해 실질적인 이미지를 요청
        imageManager.requestImage(for: asset, targetSize: CGSize(width: 30, height: 30), contentMode: .aspectFill, options: nil, resultHandler: { image, _ in cell.imageView?.image = image})
        return cell
    }
}

```

이렇게 코드를 진행하게 되면 에러가 발생할 것이다.

<center>
<figure>
<img src="/assets/post-img/iOS/71.png" alt="" width="80%">
</figure>
</center>

reloadData는 메인 쓰레드에서만 동작해야한다. >> operation queue 사용


```swift
override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view.
    let photoAuthorizationStatus = PHPhotoLibrary.authorizationStatus()

    switch photoAuthorizationStatus {
    case .authorized:
        print("접근 허가됨")
        self.requestCollection()
        self.tableView.reloadData()
    case .denied:
        print("접근 불허")
    case .notDetermined:
        print("아직 응답하지 않음")
        PHPhotoLibrary.requestAuthorization({ (status) in
            switch status {
            case .authorized:
                print("사용자가 허용함")
                self.requestCollection()
                OperationQueue.main.addOperation {
                    self.tableView.reloadData()
                }
            case .denied:
                print("사용자가 불허함")
            default: break
            }
        })
    case .restricted:
        print("접근 제한")
    }
}
```

## 사진첩에서 사진 삭제하기

```swift 
import UIKit
import Photos

class ViewController: UIViewController, UITableViewDataSource, UITableViewDelegate, PHPhotoLibraryChangeObserver {
    // PHphotoobserver는 library에 변화가 생기면 감지를 하는 프로토콜

    // 사진 목록을 테이블뷰에 보여줄 것
    @IBOutlet weak var tableView: UITableView!
    var fetchResult: PHFetchResult<PHAsset>!
    // 가져온 에셋을 가지고 이미지를 로드해옴
    let imageManager: PHCachingImageManager = PHCachingImageManager()
    let cellIdentifier: String = "cell"

    func requestCollection() {
        // 사진찍으면 사진이 저장되는 카메라롤의 컬렉션을 가져옴
        let cameraRoll: PHFetchResult<PHAssetCollection> = PHAssetCollection.fetchAssetCollections(with: .smartAlbum, subtype: .smartAlbumUserLibrary, options: nil)

        guard let cameraRollColletction = cameraRoll.firstObject else {
            return
        }
        let fetchOptions = PHFetchOptions()
        // 최신순으로 사진을 sort
        fetchOptions.sortDescriptors = [NSSortDescriptor(key: "creationDate", ascending: false)]
        // 그 결과를 fetchResult라는 프로퍼티로 가져옴
        self.fetchResult = PHAsset.fetchAssets(in: cameraRollColletction, options: fetchOptions)
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        let photoAuthorizationStatus = PHPhotoLibrary.authorizationStatus()

        switch photoAuthorizationStatus {
        case .authorized:
            print("접근 허가됨")
            self.requestCollection()
            self.tableView.reloadData()
        case .denied:
            print("접근 불허")
        case .notDetermined:
            print("아직 응답하지 않음")
            PHPhotoLibrary.requestAuthorization({ (status) in
                switch status {
                case .authorized:
                    print("사용자가 허용함")
                    self.requestCollection()
                    OperationQueue.main.addOperation {
                        self.tableView.reloadData()
                    }
                case .denied:
                    print("사용자가 불허함")
                default: break
                }
            })
        case .restricted:
            print("접근 제한")
        }
        // 아래 한줄을 써줌으로써 photoLibrary가 변화될때마다 delegate메서드가 호출됨
        PHPhotoLibrary.shared().register(self)
    }

    func tableView(_ tableView: UITableView, canEditRowAt indexPath: IndexPath) -> Bool {  // row를 편집할수 있게 해주는 메서드, tableview row를 밀어서 삭제하는 행위를 가능하게 함
        return true
    }

    func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCell.EditingStyle, forRowAt indexPath: IndexPath) {
        // 삭제 모드로 들어왔을때(편집을 하려할때)
        if editingStyle == .delete { // 삭제라면 asset을 가지고 delete를 하면
            let asset: PHAsset = self.fetchResult[indexPath.row]
        PHPhotoLibrary.shared().performChanges({PHAssetChangeRequest.deleteAssets([asset] as NSArray)}, completionHandler: nil)
            // 실제 삭제를 하게되면 나오게 되는 팝업창
        }
    }

    func photoLibraryDidChange(_ changeInstance: PHChange) {
        // observer 프로토콜을 따른 메서드

        guard let changes = changeInstance.changeDetails(for: fetchResult) else {
            return
        }
        // 어떤게 바꼈는지
        fetchResult = changes.fetchResultAfterChanges
        // 바꼈다면 tableview를 다시 불러달라
        OperationQueue.main.addOperation {
            self.tableView.reloadSections(IndexSet(0...0), with: .automatic)
        }
    }
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return self.fetchResult?.count ?? 0
    }

    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        // cell에 이미지를 하나씩 넣어줌
        let cell: UITableViewCell = tableView.dequeueReusableCell(withIdentifier: self.cellIdentifier, for: indexPath)
        // asset은 fetchresult에서 index에 해당
        let asset: PHAsset = fetchResult.object(at: indexPath.row)
        // imageManager를 통해 실질적인 이미지를 요청
        imageManager.requestImage(for: asset, targetSize: CGSize(width: 30, height: 30), contentMode: .aspectFill, options: nil, resultHandler: { image, _ in cell.imageView?.image = image})
        return cell
    }
}
```

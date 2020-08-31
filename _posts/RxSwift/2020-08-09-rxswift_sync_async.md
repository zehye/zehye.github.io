---
layout: post
title: Rxswift 동기(sync)와 비동기(async)
category: RxSwift
tags: [RxSwift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

결국 돌아돌아 다시 Rxswift를 공부하게 되었습니다.....<br>
곰튀김님 영상 Rxswift 4시간안에 끝내기 시즌2 유투브 영상을 보며 공부하였습니다.

공부에 필요한 자료는 곰튀김님 깃헙에서 클론해 가지고 왔습니다 > [곰튀김님 Github 바로가기]
(https://github.com/iamchiwon/RxSwift_In_4_Hours)

클론 후 로컬에는 rxswift가 깔려있지 않을테니 프로젝트 실행 전 `pod install` 잊지맙시다.

## 동기와 비동기

우선 개념설명에 앞서 코드를 먼저 보여드립니다.

```swift
import RxSwift
import SwiftyJSON
import UIKit

// 이미지 json이 담겨있는 URL
let MEMBER_LIST_URL = "https://my.api.mockaroo.com/members_with_avatar.json?key=44ce18f0"

class ViewController: UIViewController {
    @IBOutlet var timerLabel: UILabel!  
    @IBOutlet var editView: UITextView!

    override func viewDidLoad() {
        super.viewDidLoad()
        Timer.scheduledTimer(withTimeInterval: 0.1, repeats: true) { [weak self] _ in
            self?.timerLabel.text = "\(Date().timeIntervalSince1970)"
        }
    }

    private func setVisibleWithAnimation(_ v: UIView?, _ s: Bool) {
        guard let v = v else { return }
        UIView.animate(withDuration: 0.3, animations: { [weak v] in
            v?.isHidden = !s
        }, completion: { [weak self] _ in
            self?.view.layoutIfNeeded()
        })
    }

    // MARK: SYNC

    // load 버튼이 줄어들면서 indigator가 나와 빙빙 돈다
    @IBOutlet var activityIndicator: UIActivityIndicatorView!

    @IBAction func onLoad() {
        // 로드버튼 눌리면 뱅뱅이가 나오고
        editView.text = ""
        self.setVisibleWithAnimation(self.activityIndicator, true)

        // 동시에 진행될 수 있도록 global()
        DispatchQueue.global().async {

            // 다운로드해서
            let url = URL(string: MEMBER_LIST_URL)!
            let data = try! Data(contentsOf: url)
            // json 가져와
            let json = String(data: data, encoding: .utf8)

            DispatchQueue.main.async {
                // 가져온 json을 text에 넣고
                self.editView.text = json
                // indigator를 사라지게 한다
                self.setVisibleWithAnimation(self.activityIndicator, false)
            }
        }
    }
}
```

즉, 비동기 작업이란 것은 다른 쓰레드를 사용해서 현재 내가 작업하고 있는 작업은 작업대로 진행하고 다른 쓰레드에서 내가 원하는 작업을 비동기적으로 동시에 수행한 다음에 (멀티쓰레드로 작업한 다음) 그 결과를 비동기적으로 받아서 처리하는 코드

즉, 다른쓰레드에서 멀티쓰레드로 일을 처리한 다음에 그 결과를 비동기적으로 받아서 처리하는 코드

그러면 

---
layout: post
title: FloatingPanel 사용해보기
category: PROJECT
tags: [PROJECT]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

## FloatingPanel 사용해보기

* 참고할 Github [SCENEE Github](https://github.com/SCENEE/FloatingPanel)

우선 자신의 프로젝트를 만들고 cocoapod에 FloatingPanel을 install해준다.

### Podfile

```
pod 'FloatingPanel'
```

그리고 ContentVC 파일을 하나 만들어준다.


### ContentVC

```swift
import UIKit

class ContentVC: UIViewController, UITableViewDelegate, UITableViewDataSource {

    @IBOutlet var tableView: UITableView!
    let data = [
        "NewYork",
        "Seoul",
        "Toronto",
        "Boston",
        "Paris",
        "LA",
    ]

    override func viewDidLoad() {
        super.viewDidLoad()
        // 커스텀 셀을 만들어준다
        tableView.register(UITableViewCell.self, forCellReuseIdentifier: "cell")
        tableView.delegate = self
        tableView.dataSource = self
    }


    // data의 수만큼 행이 만들어진다.
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return data.count
    }

    // 해당 셀에 들어갈 데이터를 지정해준다.
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
        cell.textLabel?.text = data[indexPath.row]
        return cell
    }
}
```

위 코드를 보면 알겠지만, mainVC위로 올라올 tableView를 만들어주는 것으로 tableView에 대한 코드들이 작성되어있다. <br>
[UITableview.register 참고한 블로그 ](https://bonoogi.postype.com/post/4798772)


### MainVC

```swift
import UIKit
import FloatingPanel

class MainVC: UIViewController, FloatingPanelControllerDelegate {

    override func viewDidLoad() {
        super.viewDidLoad()

        // FloatingPanelController를 하나 생성해준다
        let fcp = FloatingPanelController()
        fcp.delegate = self

        // storyboardID가 FPC_content인 컨트롤러가 있는지 없는지 확인
        guard let contentViewcontroller = storyboard?.instantiateViewController(identifier: "FPC_content") as? ContentVC else {
            return
        }

        // contentViewController 설정
        fcp.set(contentViewController: contentViewcontroller)

        //`FloatingPanelController` 개체가 관리하는 뷰를 self.view에 추가하고 표시
        fcp.addPanel(toParent: self)
    }
}
```

### Storyboard

<center>
<figure>
<img src="/assets/post-img/project/3.png" alt="" width="80%">
</figure>
</center>

**주의해야할 점**

1. storyboardID를 반드시 작성할 것
2. tableView cell을 끌어다놓지 않았다는 점
3. 주의해야할 점은 아니지만 MainVC에 MapViewKit를 사용했다는 점



## 결과

<center>
<figure>
<img src="/assets/post-img/project/4.png" alt="" width="50%">
</figure>
</center>

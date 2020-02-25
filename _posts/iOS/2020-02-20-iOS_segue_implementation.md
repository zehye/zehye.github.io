---
layout: post
title: 세그(Segue)를 통한 화면 전환시 데이터 전달해보기(연습)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 세그(Segue)를 통한 화면 전환시 데이터 전달해보기


1.[Editor] > [Embed In] > [Navigation Controller] 선택

<center>
<figure>
<img src="/assets/post-img/iOS/58.png" alt="" width="80%">
</figure>
</center>


2.view controller 추가 생성 후 해당 뷰 컨트롤러에 대한 클래스 파일 생성

<center>
<figure>
<img src="/assets/post-img/iOS/59.png" alt="" width="80%">
</figure>
</center>


3.셀을 클릭하면 SecondViewController로 넘어갈 수 있도록 설정

<center>
<figure>
<img src="/assets/post-img/iOS/60.png" alt="" width="80%">
</figure>
</center>


4.새로 생성한 SecondViewController의 클래스 명을 지정후 해당 뷰 컨트롤러에 레이블 하나 추가

<center>
<figure>
<img src="/assets/post-img/iOS/61.png" alt="" width="80%">
</figure>
</center>

5.셀의 텍스트가 레이블에 넘어갈 수 있도록 레이블에 해당하는 outlet 작성후 드래그 연결

<center>
<figure>
<img src="/assets/post-img/iOS/62.png" alt="" width="80%">
</figure>
</center>

```swift

import UIKit

class SecondViewController: UIViewController {

    var textToSet: String?
    @IBOutlet weak var textLabel: UILabel!
}
```

6.아래의 코드를 참고하여 세그를 코드로 구현 > viewController에 작성

```swift
// MARK: - Navigation

// In a storyboard-based application, you will often want to do a little preparation before navigation
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    // Get the new view controller using segue.destination.
    // Pass the selected object to the new view controller.
}
```

위 코드의 의미 > 스토리보드를 사용한 애플리케이션에서 네비게이션 되기 전에 준비해야될 것이 있을 것이다. segue.destination을 사용해 다음 뷰 컨트롤러를 가져올 수 있고 그곳에 원하는 내용을 보내줄 수 있다.


```swift
// MARK: - Navigation

// In a storyboard-based application, you will often want to do a little preparation before navigation
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    // Get the new view controller using segue.destination.
    // Pass the selected object to the new view controller.
    guard let nextViewController: SecondViewController = segue.destination as? SecondViewController else {
        return
    }

    guard let cell: UITableViewCell = sender as? UITableViewCell else {
        return  // 네이게이션의 흐름을 만드는 sender
    }

    nextViewController.textToSet = cell.textLabel?.text  // 셀에 해당하는 텍스트를 다음화면으로 전달
//        nextViewController.textLabel.text = cell.textLabel?.text
}
```

7.뷰가 나타나기 전에 레이블에 글자가 보이도록 코드 작성 > SecondViewController에 작성

```swift
import UIKit

class SecondViewController: UIViewController {

    var textToSet: String?
    @IBOutlet weak var textLabel: UILabel!

    override func viewWillAppear(_ animated: Bool) {  // 화면이 보여지기 전에 레이블에 이미 글자가 셋팅이 되어있어야 함
        super.viewWillAppear(animated)
        self.textLabel.text = self.textToSet
    }

    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.
    }
}
```

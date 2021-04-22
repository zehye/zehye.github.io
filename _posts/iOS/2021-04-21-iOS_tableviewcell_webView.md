---
layout: post
title: iOS tableview cell에서 WKWebView 사용해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## WKWebView 사용해보기

현재 만들고 있는 프로젝트의 골격은 이렇다.

1. tap page Controller를 사용
2. 각 페이지는 tableview로 구성되어있으며
3. 웹뷰를 가져오는 공간은 tableview cell 안에 존재한다.

참고한 블로그는 [요기](https://stratum-home.tistory.com/entry/IOS-Swift-%EC%9B%B9%EB%B7%B0-%EC%98%88%EC%A0%9C)이며 이를 내 플젝에 맞게 재 구성하였다.


### 시작해보기

이를 시작하기 위해 나는 아래와 같은 셋팅을 했다.

1. xib 커스텀 셀 제작 > 그렇다면 당연히 tableview cell 파일이 존재할 것이며
2. MainTableVC에서 tableview에 대한 설정을 해주었다.


### MainTableVC

```swift
import UIKit
import WebKit

class MainTableVC: UIViewController {
  @IBOutlet weak var tableView: UITableView!

  override func viewDidLoad() {
        super.viewDidLoad()

        self.tableView.delegate = self
        self.tableView.dataSource = self
        self.tableView.separatorStyle = .none
        self.tableView.showsHorizontalScrollIndicator = false

        // 커스텀 셀을 가져오는 부분
        self.tableView.register(UINib(nibName: "Cell", bundle: nil), forCellReuseIdentifier: "")
    }
}

extension MainTableVC: UITableViewDelegate, UITableViewDataSource {

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 1
    }

    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

        let cell = tableView.dequeueReusableCell(withIdentifier: "셀 identifier") as! TableViewCell
        let registerCellView = cell.setWebView(rootVC: self, frame: self.view.frame)
        registerCellView.uiDelegate = self
        registerCellView.navigationDelegate = self
        cell.addSubview(registerCellView)
        return cell

    }

    func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        return UITableView.automaticDimension
    }
}

extension MainTableVC: WKUIDelegate, WKNavigationDelegate {      
    //모달창 닫힐때 앱 종료현상 방지
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }

    // alert
    func webView(_ webView: WKWebView, runJavaScriptAlertPanelWithMessage message: String, initiatedByFrame frame: WKFrameInfo, completionHandler: @escaping () -> Void) {
        let alertController = UIAlertController(title: "", message: message, preferredStyle: .alert)
        alertController.addAction(UIAlertAction(title: "확인", style: .default, handler: {(action) in completionHandler()}))
        self.present(alertController, animated: true, completion: nil)
    }

    // confirm
    func webView(_ webView: WKWebView, runJavaScriptConfirmPanelWithMessage message: String, initiatedByFrame frame: WKFrameInfo, completionHandler: @escaping (Bool) -> Void) {
        let alertController = UIAlertController(title: "", message: message, preferredStyle: .alert)
        alertController.addAction(UIAlertAction(title: "확인", style: .default, handler: {(action) in completionHandler(true)}))
        alertController.addAction((UIAlertAction(title: "취소", style: .default, handler: { (action) in completionHandler(false)})))
        self.present(alertController, animated: true, completion: nil)
    }

    // href="_blank" 처리
    func webView(_ webView: WKWebView, createWebViewWith configuration: WKWebViewConfiguration, for navigationAction: WKNavigationAction, windowFeatures: WKWindowFeatures) -> WKWebView? {
        if navigationAction.targetFrame == nil {
            webView.load(navigationAction.request)
        }
        return nil
    }
}
```

**셀 이름을 적어줘야하는 부분(withIdentifier)엔 각자가 지정해준 identifier를 적어주면 됩니다**<br>
이름을 잘 적었는지 확인하는것을 필! 수!

### cell 파일

```swift
import UIKit
import WebKit

class TableViewCell: UITableViewCell {

    override func awakeFromNib() {
        super.awakeFromNib()
        // Initialization code
    }

    func setWebView(rootVC: UIViewController, frame: CGRect) -> WKWebView {
        let webView = WKWebView(frame: self.contentView.frame)

        let url = URL(string: "가져올 웹 페이지 주소")
        let request = URLRequest(url: url!)

        webView.configuration.preferences.javaScriptCanOpenWindowsAutomatically = true
        webView.load(request)

        return webView
    }
}
```

**해당 cell파일에서 만든 setWebView 함수를 MainTableVC에서 사용하고 있음을 확인해주세요**

이렇게 테이블 뷰 셀에 웹 페이지를 가져오는 것을 해보았습니다:)<br>
잘못된 부분이 있었거나, 더 좋은 방법이 있다면 아래 댓글에 꼭꼭 남겨주세요! 

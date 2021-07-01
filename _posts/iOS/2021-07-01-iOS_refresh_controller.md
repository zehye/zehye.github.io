---
layout: post
title: UIRefresh controller 사용해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## UIRefresh Controller

리스트 형식의 테이블 뷰를 사용할때 새로고침 버튼을 두는것보다는 테이블 뷰를 아래로 당김으로써 새로고침을 진행하는 것이 좋다고 한다. <br>
실제로 우리가 많은 앱을 사용하다 보면 앱의 최 상단을 위에서 아래로 잡아당겼을 경우 새로고침이 되는 경우가 많음을 쉽게 떠올릴 수 있을 것이다.


```swift
@available(iOS 6.0, *)
open class UIRefreshControl : UIControl {


    /* The designated initializer
     * This initializes a UIRefreshControl with a default height and width.
     * Once assigned to a UITableViewController, the frame of the control is managed automatically.
     * When a user has pulled-to-refresh, the UIRefreshControl fires its UIControlEventValueChanged event.
     *
    */
    public init()


    open var isRefreshing: Bool { get }


    open var tintColor: UIColor!

    open var attributedTitle: NSAttributedString?


    // May be used to indicate to the refreshControl that an external event has initiated the refresh action
    @available(iOS 6.0, *)
    open func beginRefreshing()

    // Must be explicitly called when the refreshing has completed
    @available(iOS 6.0, *)
    open func endRefreshing()
}
```

UIRefreshControl 내부를 살펴보면 iOS6.0 버전 이상부터 지원이 되고있으며 UIControl를 상속받고 있음을 볼 수 있다. <br>
**isRefreshing** 은 현재 리프레쉬가 진행중인지를 나타나며 **tintColor** 를 통해 색의 변화도 줄 수 있다.<br>
**attributedTitle** 를 통해 새로고침 indicator 아래에 다른 글귀를 줄 수도 있다

- beginRefreshing: 새로고침이 시작됐음을 알려준다
- endRefreshing: 새로고침이 끝났음을 알려준다



### 실제 구현해보기

테이블뷰가 구현되어있는 뷰 컨트롤러로 가봅시다.


```swift
class HomeVC: UIViewController {
    @IBOutlet weak var tableView: UITableView!

    override func viewDidLoad() {
        super.viewDidLoad()
        initUI()
        initRefresh()
    }

    func initUI() {
        self.tableView.delegate = self
        self.tableView.dataSource = self
    }

    func initRefresh() {
        let refresh = UIRefreshControl() // UIRefreshControl 인스턴스화
        refresh.addTarget(self, action: #selector(updateTableView(refresh:)), for: .valueChanged)  // target 설정
        if #available(iOS 10.0, *) {
            tableView.refreshControl = refresh
        } else {
            tableView.addSubview(refresh)
        }
    }

    @objc func updateTableView(refresh: UIRefreshControl) {
        refresh.endRefreshing()
        tableView.reloadData()
    }
}
```

구체적인 테이블 뷰에 들어가는 데이터는 생략하였습니다.

UIRefreshControl를 인스턴스화 하여 target 설정을 해주면 스와이프 액션이 취해졌을 때, 연결해놓은 updateTableView 함수를 호출합니다. <br>
updateTableView함수가 호출되면 리프레쉬를 종료하는 endRefreshing 함수를 호출함으로써 <br>
initRefresh 함수는 종료하며 tableView는 reloadData를 하게 됩니다.

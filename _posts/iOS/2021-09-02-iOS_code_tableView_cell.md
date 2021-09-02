---
layout: post
title: iOS 코드로 UITableView 구현해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 코드로 UITableView 구현

개인적으로 스토리보드를 애용해 코드로 UI를 짜본적은 없었는데, 회사에서는 UI를 모두 코드로 진행하고 있었다.<br>
그중 좀 흥미로웠던 부분이 있어 정리해보려고 합니당.

코드로 테이블뷰를 만드는것에 있어서 저는 기본 뷰컨트롤러에 테이블뷰를 추가하고, 테이블 뷰 셀을 가져오는 형식으로 구현할 것입니다.


```swift
private let tableView: UITableView = {
   let tableView = UITableView(frame: .zero, style: .grouped)
   tableView.register(PracticeChatTableViewCell.self, forCellReuseIdentifier: PracticeChatTableViewCell.identifier)
   tableView.separatorStyle = .none
   tableView.rowHeight = UITableView.automaticDimension
   tableView.estimatedRowHeight = 150
   return tableView
}()
```

뷰 컨트롤러에 변수로 테이블뷰를 만드는 방식이다.<br>
이렇게 변수로 만들어놓은 테이블뷰를 화면에 위치 시켜야겠죠? 저는 snapKit을 사용했습니다.

```swift
func initViews() {
    super.initViews()

    self.view.addSubview(self.tableView)
    self.tableView.make { (make) in
        make.edges.equalToSuperview()
    }

    self.tableView.delegate = self
    self.tableView.dataSource = self
}
```

이렇게 만든 initView를 viewDidLoad에서 호출해주는 것 입니다. <br>
그리고 만들어놓은 테이블뷰 변수는 addSubview를 통해 superView와 같은 크기로 위치시켜줍니다.

그리고 웨에 적인 PracticeChatTableViewCell 파일을 만들어줍니다.

```swift
import UIKit

class PracticeChatTableViewCell: UITableViewCell {
    static let identifier = "PracticeChatCell"

    private let containerView: UIView = {
        let containerView = UIView()
        containerView.backgroundColor = .red
        return containerView
    }()

    override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)

        self.contentView.addSubview(self.containerView)
        self.containerView.make { (make) in
            make.edges.equalToSuperview()
        }
    }

    required init?(coder: NSCoder) {
        fatalError("")
    }
}
```

여기서 가장 흥미로웠던 점은 셀을 만들때 init을 생성해줘야한다는 부분이었다.

일반적으로 셀 파일에서는 앞서 뷰컨트롤러에서 했듯이 initView 함수를 만들어놓고 각 객체들의 설정들을 다뤄줬었는데(스토리보드 사용시) 코드로 이를 진행할 때에는 반드시 init을 생성해줘야한다. 그 이유는 인터페이스 빌더에서는 자동으로 이 객체들을 초기화 해주지만, 코드에서는 인터페이스 빌더를 사용하는 것이 아니기 때문에 직접 초기화를 해줘야 하는 것이다. 이렇게 초기화를 해주지 않으면 아무것도 뜨지 않는다.

실제로 아무생각없이 init을 안해줬더니 정말 아무것도 뜨지 않았다..!



뷰 컨트롤러의 전체 코드는 아래와 같다.


```swift
import UIKit

final class PracticeViewController: UIViewController {
    static func instance() -> PracticeViewController? {
        return PracticeViewController()
    }

    private let tableView: UITableView = {
        let tableView = UITableView(frame: .zero, style: .grouped)
        tableView.register(PracticeChatTableViewCell.self, forCellReuseIdentifier: PracticeChatTableViewCell.identifier)
        tableView.separatorStyle = .none
        tableView.rowHeight = UITableView.automaticDimension
        tableView.estimatedRowHeight = 150
        return tableView
    }()

    override func viewDidLoad() {
        super.viewDidLoad()
        initViews()
    }

    func initViews() {
        super.initViews()

        self.view.addSubview(self.tableView)
        self.tableView.make { (make) in
            make.edges.equalToSuperview()
        }

        self.tableView.delegate = self
        self.tableView.dataSource = self
    }
}

extension PracticeViewController: UITableViewDelegate, UITableViewDataSource {
    func numberOfSections(in tableView: UITableView) -> Int {
        return 1
    }

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 1
    }

    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: PracticeChatTableViewCell.identifier, for: indexPath)
        return cell
    }
}
```

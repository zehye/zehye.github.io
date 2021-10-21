---
layout: post
title: iOS Grouped로 지정된 tableview의 top 여백 지우는 방법
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Grouped Tableview

그룹으로 지정한 테이블뷰의 top 부분의 여백이 나오는 경우가 있다.<br>
해당 여백은 아래 코드로 지우기 가능하다.

```swift
private let tableView: UITableView = {
    let tableView = UITableView(frame: .zero, style: .grouped)
    tableView.tableHeaderView = UIView(frame: CGRect(x: 0, y: 0, width: 0, height: 0.1))
    return tableView
}()
```

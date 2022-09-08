---
layout: post
title: iOS UITableView bottom에 공간 만들어주기 
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## UITableView bottom에 공간 만들어주기

개발하는 도중에 테이블뷰 마지막 셀에 빈 공간을 추가해주고 싶었습니다.<br>
이럴때는 바로 아래와 같이 하면 됩니다. 

```swift
self.tableView.contentInset.bottom = 81
```


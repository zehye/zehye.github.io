---
layout: post
title: iOS Tap Page Controller 사용방법
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Tap Page Controller

정리중입니다.

```swift
import UIKit
import SnapKit

enum ProjectType {
    case Coupon
    case Identifier
    case Resister
}

class MainVC: UIViewController, MainVCDelegate {

    @IBOutlet weak var containerView: UIView!

    var tableType: ProjectType = .Coupon

    weak var tabPageViewController: TabPageViewController!

    var couponView = MainTableVC.instance()
    var identifierView = MainTableVC.instance()
    var resisterView = MainTableVC.instance()

    var couponList = Array<ModelProject>()
    var identifierList = Array<ModelProject>()
    var resisterList = Array<ModelProject>()


    override func viewDidLoad() {
        super.viewDidLoad()

        tabPageViewController = TabPageViewController.create()

        couponView.tabHeight = tabPageViewController.option.tabHeight
        couponView.delegate = self
        couponView.projectType = .Coupon

        identifierView.tabHeight = tabPageViewController.option.tabHeight
        identifierView.delegate = self
        identifierView.projectType = .Identifier

        resisterView.tabHeight = tabPageViewController.option.tabHeight
        resisterView.delegate = self
        resisterView.projectType = .Resister

        tabPageViewController.tabItems = [(couponView, "쿠폰번호"), (identifierView, "아이디등록"), (resisterView, "쿠폰등록")]

        tabPageViewController.option.fontSize = 14
        tabPageViewController.option.tabHeight = 38
        tabPageViewController.option.currentColor = UIColor.black
//        tabPageViewController.option.defaultColor = UIColor(named: "DDDDDD")!
        tabPageViewController.option.defaultColor = UIColor.gray
        tabPageViewController.option.currentBarHeight = 3

        self.view.addSubview(tabPageViewController.view)
        self.addChild(tabPageViewController)
        self.containerView.addSubview(tabPageViewController.view)
        tabPageViewController.view.snp.makeConstraints {(make) in
            make.top.bottom.leading.trailing.equalTo(self.containerView)
        }

        tabPageViewController.didMove(toParent: self)
    }
}
```

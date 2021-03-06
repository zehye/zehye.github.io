---
layout: post
title: iOS tableviewCell에 광고 넣어보기(Adfit)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## iOS tableviewCell에 광고 넣어보

이번 프로젝트를 하면서 앱 안에 광고를 넣는 작업을 하게 되었는데, 사용한 방법은 아래와 같습니다.

1. tableViewCell을 이용
2. 광고는 카카오에서 제공하는 Adfit을 사용

보여지는 화면 자체를 테이블뷰로 구성하였고 따라서 앱 하단에 들어가는 배너광고 또한 테이블 뷰 셀에 넣는 방식을 사용하였습니다 :)<br>
정확히는 테이블 뷰 셀 위에 uiView를 놓고 그 위에 addSubview를 하는 방식입니다.


### ViewController

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
  let cell = tableView.dequeueReusableCell(withIdentifier: "AdFit") as! AdfitTableViewCell
            let adCellView = cell.setAD(rootVC: self, frame: cell.bounds)
            adCellView.delegate = self
            cell.addSubview(adCellView)

            return cell
}
```


### TableViewCell

```swift
func setAD(rootVC: UIViewController, frame: CGRect) -> AdFitBannerAdView {
    let adView = AdFitBannerAdView(clientId: "", adUnitSize: "")
    adView.frame = CGRect(x: 0, y: 0, width: contentView.bounds.width, height: )
    adView.rootViewController = rootVC
    adView.loadAd()

    return adView
}
```

## 그런데 잠깐...

혹시 앱을 실행시켰는데 광고가 제대로 된 사이즈로 나오지 않았다면, 셀에 제대로 제약을 걸어주었는지 확인합시다.<br>
uiView를 올리고 해당 뷰에 높이 설정을 제대로 해주었다면 사실 문제없이 광고가 정 사이즈로 나오겠지만, uiView를 띄우지 않고 addSubview를 했다면 셀높이에 대한 제약 조건을 걸어주어야 합니다 :)

---
layout: post
title: 다이어리 앱 만들기 - XLPagerTabStrip 라이브러리 사용해보기
category: PROJECT
tags: [PROJECT]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

다이어리 구성을 바꾸면서 `XLPagerTabStrip`을 사용해볼 기회가 생겼다.<br>
생각보다 예쁘고 간단하며 커스텀하기 편하다는 장점을 느끼게 되어 정리해보려 한다.

[XLPagerTabStrip Github](https://github.com/xmartlabs/XLPagerTabStrip)


## XLPagerTabStrip

우선 pod에 설치해보자

```
pod 'XLPagerTabStrip', '~> 9.0'
```

그리고 메인 뷰컨(부모가 될 뷰컨트롤러)와 하위뷰컨에 XLPagerTabStrip를 import해준다.

```swift
import XLPagerTabStrip
```

### Main ViewController

메인(부모) 뷰컨트롤러에서 UIViewController의 상속을 지우고 `ButtonBarPagerTapStripViewController`를 상속 및 적용해준다.

```swift
class ViewController: ButtonBarPagerTapStripViewController
```

- 스토리보드에 컬렉션뷰를 추가해주고 ButtonBarView 클래스를 상속시켜준다

<center>
<figure>
<img src="/assets/post-img/project/1.png" alt="" width="50%">
</figure>
</center>

- 스토리보드에 스크롤뷰를 원하는 사이즈에 맞게 추가하고 오토레이아웃을 잡아준다(컬렉션 뷰도 마찬가지)

<center>
<figure>
<img src="/assets/post-img/project/2.png" alt="" width="50%">
</figure>
</center>

그리고 코드는 아래와 같다.

```swift
import XLPagerTabStrip

class ParentVC: ButtonBarPagerTabStripViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    override func viewControllers(for pagerTabStripController: PagerTabStripViewController) -> [UIViewController] {
        let diary = UIStoryboard.init(name: "Diary", bundle: nil).instantiateViewController(withIdentifier: "DiaryVC") as! DiaryVC
        diary.tabName = "다이어리"


        let calendar = UIStoryboard(name: "Calendar", bundle: nil).instantiateViewController(withIdentifier: "CalendarVC") as! CalendarVC
        calendar.tabName = "달력"

        let plan = UIStoryboard(name: "Plan", bundle: nil).instantiateViewController(withIdentifier: "PlanVC") as! PlanVC
        plan.tabName = "일정"

        return [calendar, plan, diary]
    }
}
```

- Main ViewController에서 다음과 같이 Paging될 화면과 뷰컨 재활용으로 화면 표시한다.
- 반드시 viewController함수를 오버라이딩해서 표시해주어야 한다.
  - 그렇지 않으면 쓰레드에러가 발생한다.
- Child ViewController의 내용은 메인뷰컨의 스크롤뷰에 적용되어 표시해야한다.



### Child ViewController

메인 뷰가 아닌 하위 뷰에는 아래와 같은 코드를 추가해준다.

```swift
import XLPagerTabStrip

class PlanVC: UIViewController, IndicatorInfoProvider {
    var tabName: String = ""
    @IBOutlet weak var tabNameLbl: UILabel!

    override func viewDidLoad() {
        super.viewDidLoad()
        tabNameLbl.text = tabName
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }

    func indicatorInfo(for pagerTabStripController: PagerTabStripViewController) -> IndicatorInfo {
        return IndicatorInfo(title: "\(tabName)")
    }
}
```

- IndicatorInfoProvider는 PagerTab에 나올 이름과 뷰컨을 재활용하여 화면에 보여준다.
- func indigator는 각 화면에 맞는 Label값을 인디케이터 이름으로 리턴한다.

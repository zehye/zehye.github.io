---
layout: post
title: iOS Custom Calendar 직접 만들어보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Custom Calendar 직접 만들어보기

일반적으로 우리가 달력을 구현할 때는 fsCalendar라는 라이브러리 사용을 많이 합니다. <br>
사실 결론적으로 저도 프로젝트를 진행하면서 결국은 fsCalendar를 사용했지만 초반에는 커스텀으로 직접 달력을 구현해보려고 했었습니다.

그 당시에 만들었던 코드를 공유하고자 합니다. :)


우선 당시 제 프로젝트 구조는 아래와 같습니다.

1. 뷰컨 위에 테이블뷰가 있다.
2. 테이블 뷰 셀 안에 컬렉션뷰가 존재한다.
3. 컬렉션뷰로 달력을 구현한다.

따라서 컬렉션 뷰 셀 코드는 아래와 같습니다. 

```swift 
import UIKit

class CalendarCollectionViewCell: UICollectionViewCell {

    @IBOutlet weak var containerView: UIView!
    @IBOutlet weak var dateLbl: UILabel!
}
```

셀 안에 컨테이너 뷰로 뷰 하나를 만들어놓고 해당 뷰 내부에 라벨 하나를 넣어주었습니다. <br>
그리고 아래 코드는 테이블뷰 셀 안에서 직접 달력을 그려주는 코드들이 진행되어집니다. 



```swift 
class WeekTableViewCell: UITableViewCell {
    let now = Date()
    var cal = Calendar.current
    let dateFormatter = DateFormatter()
    var components = DateComponents()
    var weeks: [String] = ["일", "월", "화", "수", "목", "금", "토"]
    var days: [String] = []
    // 해당 월이 몇일까지 있는지
    var daysCountInMonth = 0
    // 시작일
    var weekdayAdding = 0   

    @IBOutlet weak var yearMonthLbl: UILabel!
    @IBOutlet weak var collectionView: UICollectionView!

    override func awakeFromNib() {
        super.awakeFromNib()

        initUI()
    }

    func initUI() {
        self.collectionView.delegate = self
        self.collectionView.dataSource = self
        self.collectionView.isScrollEnabled = false

        let layout = UICollectionViewFlowLayout()
        layout.scrollDirection = .vertical
        layout.minimumLineSpacing = 0
        layout.minimumInteritemSpacing = 0

        self.collectionView.collectionViewLayout = layout

        dateFormatter.dateFormat = "yyyy년 M월"
        components.year = cal.component(.year, from: now)
        components.month = cal.component(.month, from: now)
        components.day = 1

        self.calculation()
        self.collectionView.reloadData()
    }

    /*
     1 일요일 2 - 1  -> 0번 인덱스부터 1일 시작
     2 월요일 2 - 2  -> 1번 인덱스부터 1일 시작
     3 화요일 2 - 3  -> 2번 인덱스부터 1일 시작
     4 수요일 2 - 4  -> 3번 인덱스부터 1일 시작
     5 목요일 2 - 5  -> 4번 인덱스부터 1일 시작
     6 금요일 2 - 6  -> 5번 인덱스부터 1일 시작
     7 토요일 2 - 7  -> 6번 인덱스부터 1일 시작
     */
    func calculation() {
        let firstDayOfMonth = cal.date(from: components)
        // 해당 수로 변환 > 1은 일요일 ~ 7은 토요일
        let firstWeeakDay = cal.component(.weekday, from: firstDayOfMonth!)
        daysCountInMonth = cal.range(of: .day, in: .month, for: firstDayOfMonth!)!.count
        // 이 과정을 해주는 이유는
        // 예로 2020년 4월이라하면 4월 1일은 수요일, 수요일이 달의 첫 날이 됨
        // 수요일은 components의 4이기 때문에 collectionView에서 앞의 3일은 비울 필요가 있음
        // 따라서 index가 1일부터 시작할 수 있도록 해줌
        // 따라서 2-4해서 -2부터 시작하게 되어 정확히 3일후부터 1일이 시작
        weekdayAdding = 2 - firstWeeakDay

        self.yearMonthLbl.text = dateFormatter.string(from: firstDayOfMonth!)

        self.days.removeAll()

        for day in weekdayAdding...daysCountInMonth {
            // 1보다 작은경우는 비워줘야함
            if day < 1 {
                self.days.append("")
            } else {
                self.days.append(String(day))
            }
        }
    }

    @IBAction func prevBtnClicked(_ sender: UIButton) {
        components.month = components.month! - 1
        self.calculation()
        self.collectionView.reloadData()
    }


    @IBAction func nextBtnClicked(_ sender: UIButton) {
        components.month = components.month! + 1
        self.calculation()
        self.collectionView.reloadData()
    }
}

extension WeekTableViewCell: UICollectionViewDelegate, UICollectionViewDataSource, UICollectionViewDelegateFlowLayout {
    func numberOfSections(in collectionView: UICollectionView) -> Int {
        return 2
    }
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        switch section {
        case 0:
            // 요일 수는 고정
            return 7
        default:
            // 일의 수
            return self.days.count
        }
    }
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = self.collectionView.dequeueReusableCell(withReuseIdentifier: "CalendarCell", for: indexPath) as! CalendarCollectionViewCell

        switch indexPath.section {
        case 0:
            // 요일
            cell.dateLbl.text = weeks[indexPath.row]
        default:
            // 일
            cell.dateLbl.text = days[indexPath.row]
        }

        if indexPath.row % 7 == 0 {
            cell.dateLbl.textColor = .red
        } else if indexPath.row % 7 == 6 {
            cell.dateLbl.textColor = .blue
        } else {
            cell.dateLbl.textColor = .white
        }

        return cell
    }
}
```

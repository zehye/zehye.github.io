---
layout: post
title: iOS FSCalendar 써본것 모두다 싹다! 정리
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## FSCalendar 

이번에 달력을 구현함에 있어서 FSCalendar라는 라이브러리를 사용한 경험이 있었습니다. <br>
모든 기능을 다 써본것은 아니지만, 그래도 써본 모든 기능에 대해서 정리를 해보려 합니다!


### 기본 셋팅 

여기서 중요한거... delegate들 꼭.. 꼭.. 가져오세요..<br>
첨에 아니 이거 사용하려면 스택오버플로우에 이런 함수있다는데.. 대체 왜!! 왜 나는 안불러져 오는거지..?! 했는데

응~ FSCalendarDelegateAppearance 이거 채택 안했구요..ㅎ

꼭.. 꼭.. 잊지마세요!<br>
저 같은 바보같은 행동 하지마세요.흑ㅎ극...

```swift 
class CalendarViewController: UIViewController, FSCalendarDelegate, FSCalendarDataSource, FSCalendarDelegateAppearance {
    @IBOutlet weak var calendar: FSCalendar!
    var selectedDate: Date = Date()

    override func viewDidLoad() {
        super.viewDidLoad()
        setCalendarUI()
    }

    // 오늘 날짜로 돌아오는 버튼 및 액션 
    @IBOutlet weak var currentBtn: UIButton!
    @IBAction func currentBtnClicked(_ sender: Any) {
        self.calendar.select(Date.Today())
    }

    // 현재 달로 돌아오기 위해서는 
    @IBAction func currentMonthBtnClicked(_ sender: Any) {
        self.calendar.setCurrentPage(Date(), animated: true)
    }

    func setCalendarUI() {
        // delegate, dataSource 
        self.calendar.delegate = self
        self.calendar.dataSource = self
        
        // calendar locale > 한국으로 설정 
        self.calendar.locale = Locale(identifier: "ko_KR")
        
        // 상단 요일을 한글로 변경 
        self.calendar.calendarWeekdayView.weekdayLabels[0].text = "일"
        self.calendar.calendarWeekdayView.weekdayLabels[1].text = "월"
        self.calendar.calendarWeekdayView.weekdayLabels[2].text = "화"
        self.calendar.calendarWeekdayView.weekdayLabels[3].text = "수"
        self.calendar.calendarWeekdayView.weekdayLabels[4].text = "목"
        self.calendar.calendarWeekdayView.weekdayLabels[5].text = "금"
        self.calendar.calendarWeekdayView.weekdayLabels[6].text = "토"
        
        // 월~일 글자 폰트 및 사이즈 지정
        self.calendar.appearance.weekdayFont = UIFont.SpoqaHanSans(type: .Regular, size: 14)
        // 숫자들 글자 폰트 및 사이즈 지정
        self.calendar.appearance.titleFont = UIFont.SpoqaHanSans(type: .Regular, size: 16)
        
        // 캘린더 스크롤 가능하게 지정
        self.calendar.scrollEnabled = true
        // 캘린더 스크롤 방향 지정 
        self.calendar.scrollDirection = .horizontal
        
        // Header dateFormat, 년도, 월 폰트(사이즈)와 색, 가운데 정렬
        self.calendar.appearance.headerDateFormat = "YYYY년 MM월"
        self.calendar.appearance.headerTitleFont = UIFont.SpoqaHanSans(type: .Bold, size: 20)
        self.calendar.appearance.headerTitleColor = UIColor(named: "FFFFFF")?.withAlphaComponent(0.9)
        self.calendar.appearance.headerTitleAlignment = .center
        
        // 요일 글자 색
        self.calendar.appearance.weekdayTextColor = UIColor(named: "F5F5F5")?.withAlphaComponent(0.2)
        
        // 캘린더 높이 지정 
        self.calendar.headerHeight = 68        
        // 캘린더의 cornerRadius 지정 
        self.calendar.layer.cornerRadius = 10
        
        // 양옆 년도, 월 지우기
        self.calendar.appearance.headerMinimumDissolvedAlpha = 0.0
    
        // 달에 유효하지 않은 날짜의 색 지정 
        self.calendar.appearance.titlePlaceholderColor = UIColor.white.withAlphaComponent(0.2)
        // 평일 날짜 색
        self.calendar.appearance.titleDefaultColor = UIColor.white.withAlphaComponent(0.5)
        // 달에 유효하지않은 날짜 지우기 
        self.calendar.placeholderType = .none
        
        // 캘린더 숫자와 subtitle간의 간격 조정 
        self.calendar.appearance.subtitleOffset = CGPoint(x: 0, y: 4)
        
        self.calendar.select(selectedDate)
    }

    // 날짜를 선택했을 때 할일을 지정 
    func calendar(_ calendar: FSCalendar, didSelect date: Date, at monthPosition: FSCalendarMonthPosition) {
        self.navigationController?.popViewController(animated: true)
        self.delegate?.dateUpdated(date: date.key)
    }

    // 선택된 날짜의 채워진 색상 지정 
    func calendar(_ calendar: FSCalendar, appearance: FSCalendarAppearance, fillSelectionColorFor date: Date) -> UIColor? {
        return appearance.selectionColor
    }

    // 선택된 날짜 테두리 색상
    func calendar(_ calendar: FSCalendar, appearance: FSCalendarAppearance, borderSelectionColorFor date: Date) -> UIColor? {
        return UIColor.white.withAlphaComponent(1.0)
    }
    
    // 모든 날짜의 채워진 색상 지정 
    func calendar(_ calendar: FSCalendar, appearance: FSCalendarAppearance, fillDefaultColorFor date: Date) -> UIColor? {
        return UIColor.white
    }
    
    // title의 디폴트 색상 
    func calendar(_ calendar: FSCalendar, appearance: FSCalendarAppearance, titleDefaultColorFor date: Date) -> UIColor? {
        return UIColor.white.withAlphaComponent(0.5)
    }
    
    // subtitle의 디폴트 색상 
    func calendar(_ calendar: FSCalendar, appearance: FSCalendarAppearance, subtitleDefaultColorFor date: Date) -> UIColor? {
        return UIColor.white.withAlphaComponent(0.5)
    }
    
    // 원하는 날짜 아래에 subtitle 지정 
    // 오늘 날짜에 오늘이라는 글자를 추가해보았다
    func calendar(_ calendar: FSCalendar, subtitleFor date: Date) -> String? {
        switch dateFormatter.string(from: date) {
        case dateFormatter.string(from: Date()):
            return "오늘"
        default:
            return nil
        }
    }

    // 날짜의 글씨 자체를 오늘로 바꾸기 
    func calendar(_ calendar: FSCalendar, titleFor date: Date) -> String? {
        switch dateFormatter.string(from: date) {
        case dateFormatter.string(from: Date()):
            return "오늘"
        default:
            return nil
        }
    }
}
```

이 외에도 커스텀할 수 있는 다양한 요소들이 있습니다.<br>
함수를 호출해서 사용하는 방법은 여러가지이니 각자 취향에 맞게 사용하면 좋을 것 같습니다 :)
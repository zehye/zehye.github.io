---
layout: post
title: iOS DatePicker 사용해보기
category: iOS
tags: [iOS]
comments: true
---

## DatePicker 

우선 iOS14 이후부터 데이트피커의 스타일에 .inline 이 생겼더군요..<br>
기존 데이트피커 레이아웃이 정말 안예쁘다 생각했던 1인으로서 해당 레이아웃....너무 예쁜것같아요.

우선 데이트 피커를 사용하기 위해 뷰컨 위에 피커 올려놓습니다.<br>
저는 평소 코드로도 레이아웃 작업을 하지만 스토리보드를 굉장히 선호합니다.<br>
작업속도도 빠르고 눈으로도 보이고.. 이게 또 아이폰 개발자의 감성이라고 생각하거든요..

무거운것만 빼면.. 여튼 ..


뷰컨에 피커를 올려두고 간단한 ui 셋팅을 해줍니다. 


```swift 
@IBOutlet weak var datePicker: UIDatePicker!

func initUI() {
    setDatePicker()
}

func setDatePicker() {
    if #available(iOS 14.0, *) {
        datePicker.preferredDatePickerStyle = .inline
    } else {
        datePicker.preferredDatePickerStyle = .automatic
    }
    // locale 설정을 해주고
    datePicker.locale = Locale(identifier: "ko_KR")
    datePicker.calendar.locale = Locale(identifier: "ko_KR")

    // 현재 시간에 맞게 자동으로 업데이트 하게 해주고 
    datePicker.timeZone = .autoupdatingCurrent

    // 피커에 달력과 시간을 설정하는 것 두개를 다 사용할 것입니다. 
    datePicker.datePickerMode = .dateAndTime
    datePicker.sizeToFit()

    // 피커의 데이터를 선택했을 때 어떤 동작을 하게 할지 addTarget도 추가해볼 수 있습니다. 
    datePicker.addTarget(self, action: #selector(handleDatePicker(_:)), for: .valueChanged)

    // 선택된 날짜, 주된 컬러를 설정
    datePicker.tintColor = ColorTheme(foreground: .green)
    
    // 오늘로부터 과거 날짜 블러처리
    datePicker.minimumDate = Date()
}

// 피커의 데이터를 선택했을 때 어떤 행위를 할지 정의해주는 함수 
@objc func handleDatePicker(_ sender: UIDatePicker) {
    print(sender.date)
}
```

저는 여기서 추가로 하고싶던 동작이 있었는데요.<br>
화면위에 이 데이트 피커가 담긴 뷰컨을 위에 띄우고 이 뷰컨을 팝업처럼 사용하고 싶었습니다. 

한마디로 vc1위에 vc2(=datePickerVC)를 띄워 팝업처럼 사용한다.<br>
그러려면 vc2의 배경은 딤처리가 되어있어야 할것이고, 배경을 눌렀을때에도 화면을 나갈 수 있어야 합니다.

이를 위한 코드로 tapGesture를 사용했습니다.

```swift 
override func viewDidLoad() {
    super.viewDidLoad()
    self.view.backgroundColor = UIColor.clear
}

func initUI() {
    let tapGesture = UITapGestureRecognizer()
    tapGesture.delegate = self
    self.backView.addGestureRecognizer(tapGesture)
    
    setDatePicker()
}

func gestureRecognizer(_ gestureRecognizer: UIGestureRecognizer, shouldReceive event: UIEvent) -> Bool {
    self.dismiss(animated: false)
    return true
}
```

해당 뷰컨에 `UIGestureRecognizerDelegate` 프로토콜을 채택하는거 잊지 마시구욤<br>
저는 데이트 피커 뒤에 backView라는 뷰를 올려놓고 해당 뷰에 bgColor를 black + alpha 0.5를 준 상태입니다.

그리고 뷰컨의 contentView의 bgColor는 clear를 한 상태이구요.

이러면 일단 vc1에서 vc2를 띄웠을때 dim 처리가 되어있는 상태로 보이게 될 것입니다.<br>
그리고 tapGesture를 통해서 바깥 backView를 터치했을때 해당 뷰컨이 dismiss 될수 있도록 보여주었구요.

그럼 반대로 피커 위에 버튼을 하나 만들어놓고(=저장버튼) 해당 버튼을 눌렀을 때의 행위를 정의해보겠습니다.


```swift 
@IBOutlet weak var saveBtn: UIButton!
    
@IBAction func saveBtnClicked(_ sender: Any) {
    // datePicker의 format 형식을 정의해줍니다. 
    let dateFormatter = DateFormatter()
    dateFormatter.dateFormat = "yyyy-MM-dd   a h시 mm분"
    dateFormatter.amSymbol = "오전"
    dateFormatter.pmSymbol = "오후"
    
    dateFormatter.string(from: self.datePicker.date)
    self.dismiss(animated: false)
}
```

근데 저는 한가지 특이점이 있었는데요.<br>
만약 데이트피커가 있는 뷰컨에서 저장을 하고 나왔을때, 같은 뷰컨에 있는 라벨 등의 컴포넌트에 변화를 주고싶다면 단순히 `self.dateLbl.text = datePicker.date` 이런식으로 할수 있었을 것입니다.

근데 저는 이 피커를 현재 팝업처럼 사용하고 있었죠?

그렇기 때문에 vc2의 데이터를 vc1으로 가져와서 화면 변화를 보여주어야 합니다.

즉 데이트피커 뷰컨(=vc2)에서 데이트를 선택하고 나면 해당 값을 vc1에서 보여주게 되어야 한다는것입니다.<br>
이는 어떻게 처리해야할까요?

바로 `Delegate`를 사용하는 것입니다. 

데이트 피커가 있는 뷰컨에서


```swift
protocol DateTimePickerVCDelegate: AnyObject {
    func updateDateTime(_ dateTime: String)
}
``` 
이렇게 프로토콜 하나를 선언해주고

```swift 
final class DateTimePickerViewController: UIViewController {


@IBOutlet weak var datePicker: UIDatePicker!
@IBOutlet weak var saveBtn: UIButton!

weak var delegate: DateTimePickerVCDelegate?

@IBAction func saveBtnClicked(_ sender: Any) {
    let dateFormatter = DateFormatter()
    dateFormatter.dateFormat = "yyyy-MM-dd   a h시 mm분"
    dateFormatter.amSymbol = "오전"
    dateFormatter.pmSymbol = "오후"
    
    dateFormatter.string(from: self.datePicker.date)
    
    // 해당 버튼을 눌렀을때 delegate 동작할 수 있도록 합니다. 
    self.delegate?.updateDateTime(dateFormatter.string(from: self.datePicker.date))
    self.dismiss(animated: false)
}
```

그럼 vc1에서 이 프로토콜을 정의해주어야 하겠죠

```swift
extension BaseViewController: DateTimePickerVCDelegate {
    var dateTime: String?

    func updateDateTime(_ dateTime: String) {
        self.dateTime = dateTime
        self.tableView.reloadData()
    }
}
```

테이블뷰 리로드를 해줬던 이유는 다른게 아니라, 제가 해당 레이아웃 개발을 테이블뷰로 진행했기 때문입니다.

즉 테이블 뷰 셀을 클릭하면 데이트피커가 있는 뷰컨(vc2)로 이동하고 셀에 있는 라벨에 변화를 주고 있는것이죠<br>
코드는 아래와 같습니다.


```swift 
// 뷰컨에서 dateTime이라는 변수 하나를 만들어 놓고 
var dateTime: String?

@objc func pushDatePickerVC(_ sender: Any) {
    let vc = DateTimePickerViewController.instance()
    // vc2의 Delegate를 self로 연결해줍니다. 
    vc.delegate = self
    self.present(vc, animated: false, presentStyle: .overFullScreen)
}

func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    if row == 0 {
        // cell 연결해주고 
        guard let cell = tableView.dequeueReusableCell(withIdentifier: "BaseTableViewCell") as? BaseTableViewCell else { fatalError() }
        
        // 셀 내부의 버튼을 누르면 datePickerVC로 넘어가도록 addTarget 
        cell.dateTimeBtn.addTarget(self, action: #selector(pushDatePickerVC(_:)), for: .touchUpInside)
        
        // 현 뷰컨의 dateTime이 존재한다면 
        if self.dateTime != nil {
            // 넣어주고 
            cell.dateTimeLbl.text = self.dateTime
        } else {
            // 그렇지않다면 날짜선택하라는 문구가 뜨도록 한다 
            cell.dateTimeLbl.text = "SELECT_DATE_TIME".localized
        }
        return cell
    }
}

extension BaseViewController: DateTimePickerVCDelegate {
    func updateDateTime(_ dateTime: String) {
        // 해당 dateTime은 vc2(datePickerVC)로부터 받아온다 
        self.dateTime = dateTime
        self.tableView.reloadData()
    }
}
```


---
layout: post
title: PickerView가 있는 Alert 창 만들기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## PickerView가 있는 Alert 창 만들기

해당 블로그를 참고하여 만들어보았습니다. > [참고한 블로그](https://medium.com/@zhiyao92/ios-tutorials-pickerview-on-uialertcontroller-338cb94e30e9)


### UIAlertController

addAction에 tableView.reloadData()가 들어가는 이유는 해당 버튼이 tableview cell에 들어가있기 때문이다. <br>

1. 버튼을 누르면 개월수를 체크하는 pickerView가 뜬다.
2. 원하는 개월수를 선택 후 확인 버튼을 누르면 label에 내가 선택한 개월수가 나온다.

```swift
class ViewController: UIViewController {

    let pickerList = ["2개월", "3개월", "4개월", "5개월", "6개월", "7개월", "8개월", "9개월", "10개월", "11개월", "12개월"]
    var pickerView = UIPickerView()
    var typeValue = String()

    @IBAction func touchDateSelectBtn(_ sender: UIButton) {
        let alert = UIAlertController(title: "예상 프로젝트 기간", message: "\n\n\n\n\n\n", preferredStyle: .alert)
        alert.isModalInPopover = true
        let pickerFrame = UIPickerView(frame: CGRect(x: 5, y: 20, width: 250, height: 140))
        alert.view.addSubview(pickerFrame)
        pickerFrame.delegate = self
        pickerFrame.dataSource = self

        alert.addAction(UIAlertAction(title: "취소", style: .default, handler: nil))
        alert.addAction(UIAlertAction(title: "확인", style: .default, handler: { (UIAlertAction) in
            print("\(self.typeValue)")
            self.tableView.reloadData()
        }))
        self.present(alert, animated: true, completion: nil)

    }
```

### UIPickerViewDelegate, UIPickerViewDataSource


```swift
extension ViewController: UIPickerViewDelegate, UIPickerViewDataSource {
    func numberOfComponents(in pickerView: UIPickerView) -> Int {
        return 1
    }

    func pickerView(_ pickerView: UIPickerView, numberOfRowsInComponent component: Int) -> Int {
        return self.pickerList.count
    }

    func pickerView(_ pickerView: UIPickerView, titleForRow row: Int, forComponent component: Int) -> String? {
        return pickerList[row]
    }

    func pickerView(_ pickerView: UIPickerView, didSelectRow row: Int, inComponent component: Int) {
        if row == 0 {
            typeValue = "2개월"
        } else if row == 1 {
            typeValue = "3개월"
        } else if row == 2 {
            typeValue = "4개월"
        } else if row == 3 {
            typeValue = "5개월"
        } else if row == 4 {
            typeValue = "6개월"
        } else if row == 5 {
            typeValue = "7개월"
        } else if row == 6 {
            typeValue = "8개월"
        } else if row == 7 {
            typeValue = "9개월"
        } else if row == 8 {
            typeValue = "10개월"
        } else if row == 9 {
            typeValue = "11개월"
        } else if row == 10 {
            typeValue = "12개월"
        }
    }
}
```

### UITableView

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
  let cell = tableView.dequeueReusableCell(withIdentifier: "DateSelect") as! DateSelectTableViewCell
    cell.pickerLbl.text = self.typeValue
    return cell
```

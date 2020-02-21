---
layout: post
title: 테이블 뷰에 동적으로 데이터 추가해보기(연습)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 테이블 뷰에 동적으로 데이터 추가해보기

실시간으로 추가되는 데이터가 테이블뷰에 반영되는 것을 만들어보자!

tableView위에 button 하나를 만들어봅니다.

<center>
<figure>
<img src="/assets/post-img/iOS/44.png" alt="" width="50%">
</figure>
</center>


그리고 해당 버튼이 수행할 액션을 코드로 작성한다.<br>
이전에 사용해봤던 dateFormatter를 사용함으로써 버튼을 누르면 날짜가 계속해서 업데이트되어 보여지는 화면을 구성해보자!


```swift
var dates: [Date] = []

let dateFormatter: DateFormatter = {
    let formatter: DateFormatter = DateFormatter()
    formatter.dateStyle = .medium
    formatter.timeStyle = .medium
    return formatter
}()

@IBAction func touchUpAddBtn(_ sender: UIButton) {
    dates.append(Date())
//        self.tableView.reloadData()  해당 섹션의 데이터만 가져오는 것이 아니라 전체 데이터를 다시 불러오게 된다 > 비효율
    self.tableView.reloadSections(IndexSet(2...2), with: UITableView.RowAnimation.automatic)
}
```

1. 변수 dates를 만들어 빈 array를 만들어준다.
2. dateFormatter를 사용
3. 해당 액션을 클릭할때마다 array에 데이터가 추가되어진다.
4. reloadSections를 사용하여 해당 섹션의 데이터만 불러오도록 하고
5. 추가되는 데이터가 애니메이션 효과가 적용된 상태로 보여지도록 한다.

이전 실습에서는 단순 섹션을 추가하고 해당 섹션에 데이터를 추가해줬기 때문에 기존 코드또한 조금 변경을 해줘야 한다.

```swift
func numberOfSections(in tableView: UITableView) -> Int {
    return 3  // section의 수를 3개로 늘려주고
}

func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    switch section {
    case 0:
        return korean.count
    case 1:
        return english.count
    case 2:  // button section에 해당하는 case를 추가해준다
        return dates.count
    default:
        return 0
    }
}

func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

    let cell: UITableViewCell = tableView.dequeueReusableCell(withIdentifier: self.cellIdentifier, for: indexPath)
    if indexPath.section < 2 {  // section이 2 이하일때 실행하도록 하고
        let text: String = indexPath.section == 0 ? korean[indexPath.row] : english[indexPath.row]
        cell.textLabel?.text = text
    } else {  // 그 외의 case에 대해 실행하는 코드를 추가해준다
        cell.textLabel?.text = self.dateFormatter.string(from: self.dates[indexPath.row])
    }

    return cell
}

func tableView(_ tableView: UITableView, titleForHeaderInSection section: Int) -> String? {
    if section < 2 {  // 해당 코드 또한 section이 2 이하일때 실행되도록 하고
        return section == 0 ? "한글" : "영어"
    }
    return nil  // 이외에는 nil을 리턴하도록 한다
}
```

작성한 코드를 실행해보면 아래와 같이 보여질 것이다!

<center>
<figure>
<img src="/assets/post-img/iOS/45.png" alt="" width="50%">
</figure>
</center>

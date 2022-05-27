---
layout: post
title: iOS CSV 파일을 파싱해서 테이블뷰에 뿌려보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## CSV 파일을 파싱해보기

csv파일을 파싱해 테이블뷰에 뿌려보려고 합니다. 

우선 csv파일은 무엇일까요?<br>
csv파일은 데이터들을 쉽표로 구분한 텍스트 데이터 혹은 텍스트 파일을 의미합니다.

확장자가 .csv인 것이죠.

예로들자면

```
우유,100,234
김밥,100,530
```

이런식으로 음식에 대한 데이터가 이름, 1회 제공량당 그램수, 1회 제공량당 칼로리 이렇게 쉽표로 구분되어져 보이는 텍스트 데이터를 의미합니다.


저는 이번에 식품의약품안전처에서 제공해주는 음식데이터들을 `이름, 1회 제공량당 그램수, 1회 제공량당 칼로리`로만 구분해놓은 csv파일을 파싱해 테이블뷰에 보여줄 예정인데요. 실제 식품의약품안전처에서는 이에 대한 정보를 api 혹은 엑셀파일을 제공해주고 있는데요. 해당 api를 통신을 통해 가져오려고 하니 너무 오랜 시간이 걸리더라구요.. 그래서 이번에는 엑셀파일을 csv로 변환하여 그 해당하는 파일을 프로젝트 내에서 파싱해 테이블뷰에서 보여줄 예정입니다.


우선 해당 엑셀파일을 csv로 변환해서(저장하는 방식을 .csv로 저장하면 쉽게 받을 수 있습니다) 받아두었고, 해당 csv파일을 엑스코드 프로젝트 내부에 옮겨놓았습니다. 

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/35.png" alt="" width="80%">
</figure>
</center>

이렇게 말이죠. 저는 Calorie라는 파일로 들어가져있습니다. 

해당 파일에는 아래와 같이 데이터들이 보여집니다. 

```
The 더 건강한 델리햄,100,230,
The 더 건강한 브런치 슬라이스 오리지널,100,120,
The 더 건강한 햄 치즈,100,330,
```

식품이름, 1회제공량당 그램수, 1회 제공량당 칼로리 이렇게 말이죠.


### 본격적으로 파싱해보기

실제로 저는 모델을 따로 만들어 두고있고, 해당 모델에서는 name, per, cal 이렇게 필드를 세개로 나누어 csv 파일 데이터들을 받아줄 예정입니다. 

```swift 
class SearchViewController: UIViewController {
    var foodList: [FoodModel] = []
}
```

이렇게 foodList라는 변수에 담아서 보여줄 예정입니다. 


```swift 
class SearchViewController: UIViewController {
    var foodList: [FoodModel] = []

    override func viewDidLoad() {
        super.viewDidLoad()

        self.loadCalorieFromCSV()
    }

    private func parseCSVAt(url: URL) {
        do {
            // url을 받은 data
            let data = try Data(contentsOf: url)
            // 해당 data를 encoding 합니다.
            let dataEncoded = String(data: data, encoding: .utf8)
            // ,로 구분해 만든 리스트를 제가 만들어 놓은 foodList에 담아줍니다. (FoodModel이라는 model에 맞게)
            if let dataArr = dataEncoded?.components(separatedBy: "\n").map({$0.components(separatedBy: ",")}) {
                self.foodList = dataArr.compactMap({ FoodModel(value: $0) })
            }
        } catch {
            print("Error reading CSV file")
        }
    }
    
    private func loadCalorieFromCSV() {
        // bundle에 있는 경로 > Calorie라는 이름을 가진 csv 파일 경로 
        let path = Bundle.main.path(forResource: "Calorie", ofType: "csv")!
        parseCSVAt(url: URL(fileURLWithPath: path))

        self.tableView.reloadData()
    }
}
```
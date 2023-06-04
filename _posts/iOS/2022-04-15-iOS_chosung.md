---
layout: post
title: iOS searchBar 초성검색 기능 추가해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## UISeachBar 초성검색 기능 추가해보기

검색바에 좀더 다채로움을 주고싶어서.. 그게 뭐가 있을까 고민해보니..<br>
초성검색 기능이 있으면 괜찮곘다 싶어졌습니다.


```swift 
let hangeul = ["ㄱ","ㄲ","ㄴ","ㄷ","ㄸ","ㄹ","ㅁ","ㅂ","ㅃ","ㅅ","ㅆ","ㅇ","ㅈ","ㅉ","ㅊ","ㅋ","ㅌ","ㅍ","ㅎ"]

func chosungCheck(word: String) -> String {
    var result = ""
    
    for char in word {
        // unicodeScalars: 유니코드 스칼라 값의 모음으로 표현되는 문자열 값
        let octal = char.unicodeScalars[char.unicodeScalars.startIndex].value
        if 44032...55203 ~= octal {
            let index = (octal - 0xac00) / 28 / 21
            result = result + hangeul[Int(index)]
        }
    }
    return result
}

func isChosung(word: String) -> Bool {
    var isChosung = false
    for char in word {
        if 0 < hangeul.filter({ $0.contains(char)}).count {
            isChosung = true
        } else {
            isChosung = false
            break
        }
    }
    return isChosung
}
```

그럼 이 코드를 서치바에서 어떻게 적용시킬까요?


```swift 
var arr = ["zehye", "hi", "hello", "nice", "to", "meet", "you"]
var filterredArr: [String] = []

extension SearchViewController: UISearchBarDelegate {   
    func searchBar(_ searchBar: UISearchBar, textDidChange searchText: String) {
        // 텍스트의 공백 제거
        let text = searchText.trimmingCharacters(in: .whitespaces)
        
        // 서치바에 적힌 공백이 제거된 글자의 초성을 체크
        let isChosungCheck = isChosung(word: text)
        
        // filterText라는 상수를 받아 arr를 필터링 합니다.
        // 서치바에서 받은 text에 arr의 초성이 포함되어있는지 여부를 따집니다. 
        let filterText = arr.filter({
            if isChosungCheck {
                return ($0.contains(text) || chosungCheck(word: $0).contains(text))
            } else {
                return $0.contains(text)
            }
        })
        
        // 포함되는 filterText를 filterredArr에 담아주고 
        self.filterredArr = filterText
        // 테이블뷰 리로드를 합니다. 
        self.tableView.reloadData()
    }
    
    // 검색버튼을 눌렀을때도 마찬가지의 로직을 진행합니다. 
    func searchBarSearchButtonClicked(_ searchBar: UISearchBar) {
        
        guard let text = self.searchBar.text?.trimmingCharacters(in: .whitespaces) else { return }
        
        let isChosungCheck = isChosung(word: text)
        
        let filterText = list.filter({
            if isChosungCheck {
                return ($0.contains(text) || chosungCheck(word: $0).contains(text))
            } else {
                return $0.contains(text)
            }
        })
        
        self.filterredArr = filterText
        self.isFiltering = !(self.searchBar.text?.count == 0)
        
        self.tableView.reloadData()
    }
}
```

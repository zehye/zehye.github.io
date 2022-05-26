---
layout: post
title: iOS SearchController 첫번째, 기본 셋팅해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## SearchController 첫번째, 기본 셋팅해보기

프로젝트에서 검색창을 구현하기 위해 searchController를 사용하게 되었습니다.<br>
그치만 결론은 앞으로는 서치바 자체만을 사용하게 될 것같다는 말씀...!

서치 컨트롤러는 사용해보니 화면상에서 서치 컨트롤러 자체가 제약을 두는 것이 많았습니다.<br>
화면에 컨트롤러 하나가 더 겹쳐지게 됨으로써 커스텀을 하는데에도 찾아봐야할 것들이 많았구요.<br>
검색을 위한 행위를 만들것이라면 그저 searchBar를 올려두고 그 안에서 해결하는것이 좀더 좋은 방법이 될 것 같다고 생각합니다..

좀더.. 간단해요! 그게...ㅎㅎ

그럼에도 서치 컨트롤러를 쓴다면! 아래와 같이 진행해보세요.


### UISearchController 

저는 서치 컨트롤러를 셋팅해주는 함수 하나를 만들어줄 예정이며, 이 함수를 viewDidLoad에서 불러서 사용할 것입니다.


```swift
func setSearchControllerUI() {
    let searchController = UISearchController(searchResultsController: nil)
    self.navigationItem.searchController = searchController
}
```

위와 같이 initialize를 해줍니다. 

서치 컨트롤러 안에 서치바가 있기 때문에 서치바와 관련된 설정 코드들을 추가해줄수도 있습니다. 

```swift 
func setSearchControllerUI() {
    let searchController = UISearchController(searchResultsController: nil)
    searchController.searchBar.placeholder = "검색창입니다"

    self.navigationItem.searchController = searchController
}
```

이때 searchResultController는 무엇일까요?

우선 이 searchResultController에는 검색결과를 표시하고 싶은 뷰컨이 들어가면 됩니다.


### 예제 코드


기본적으로 우리가 서치 컨트롤러를 사용하면 알아야할 프로퍼티들입니다. 

```swift 
func setSearchControllerUI() {
    let searchController = UISearchController(searchResultsController: nil)
    
    // set placeholder
    let placeholder = "검색창입니다"
    searchController.searchBar.placeholder = placeholder
    // searchController가 검색하는 동안 네비게이션에 가려지지 않도록 
    searchController.hidesNavigationBarDuringPresentation = false
    // searchBar cancel button text change (cancel을 취소로 텍스트 변경)
    searchController.searchBar.setValue("취소", forKey: "cancelButtonText")
    
    // placeholder릐 글꼴과 글자 크기 변경 
    let attributedString = NSMutableAttributedString(string: placeholder, attributes: [
        NSAttributedString.Key.font: UIFont(name: "Spoqa Han Sans Neo", size: 16) as Any,
        NSAttributedString.Key.foregroundColor: UIColor(white: 255/255, alpha: 0.3)
    ])
    searchController.searchBar.searchTextField.attributedPlaceholder = attributedString
    // searchBar의 textField의 배경색, 폰트, 글자크기 변경
    searchController.searchBar.searchTextField.backgroundColor = .clear
    searchController.searchBar.searchTextField.font = UIFont(name: "Spoqa Han Sans Neo", size: 16)
    
    // clear button 설정 
    if let button = searchController.searchBar.searchTextField.value(forKey: "_clearButton") as? UIButton {
        let templateImage = button.imageView?.image?.withRenderingMode(.alwaysTemplate)
        button.setImage(templateImage, for: .normal)
        button.tintColor = .white
    }
    
    // 서치 컨트롤러 왼쪽에 back button color 변경 
    searchController.searchBar.searchTextField.leftView?.tintColor = UIColor.white
    // searchBar cancel button color
    searchController.searchBar.tintColor = UIColor.white
    
    // navigation title 
    self.navigationItem.searchController = searchController
    self.navigationItem.title = "검색하기"

    // searchBar back button color
    self.navigationItem.searchController?.searchBar.barTintColor = UIColor.white

    // navigation item 스크롤 될때 사라지지 않게 
    self.navigationItem.hidesSearchBarWhenScrolling = false

    // navigation item back button > back text remove (back 문구 없애기)
    self.navigationController?.navigationBar.topItem?.backButtonDisplayMode = .minimal

    // navigation item back button 이미지 변경
    self.navigationController?.navigationBar.backIndicatorImage = UIImage(named: "icBack")
    self.navigationController?.navigationBar.backIndicatorTransitionMaskImage = UIImage(named: "icBack")
}
```
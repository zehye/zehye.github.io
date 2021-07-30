---
layout: post
title: tintColor란 무엇이며 image renderingMode 살펴보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## tintColor

tintColor란 시각적으로 화면 상의 어떤 요소가 현재 활성화되었는지를 보여주는 요소

예로들어 NavigationBar의 아이템의 refresh 버튼이나 back 버튼을 누르면 단순히 눌리고 끝나는 것이 아니라 눌려졌을때 흰색으로 살짝 변했다가 다시 원래의 색으로 돌아오는 것이 그 예시이다. 이런 효과를 가능하게 하는것이 바로 `tintColor`이다. 기본적으로 tintColor는 UIView의 프로퍼티로 존재하기에 UIView를 상속받는뷰들은 모두 tintColor를 적용시킬 수 있다!


따라서 view에 tintColor를 사용하기 위해서는 약간의 방법이 필요한데<br>
그 방법은 바로 image의 renderingMode를 `.alwaysTemplate`으로 바꿔야 하는 것이다.




### renderingMode

UIImage의 renderingMode코드를 살펴보면 아래와 같이 세가지 옵션이 있다.

```
@available(iOS 7.0, *)
    public enum RenderingMode : Int {


        case automatic = 0 // Use the default rendering mode for the context where the image is used


        case alwaysOriginal = 1 // Always draw the original image, without treating it as a template

        case alwaysTemplate = 2 // Always draw the image as a template image, ignoring its color information
    }
```

#### 1. automatic

이미지 렌더링 모드 중에서 automatic은 default 값이다.<br>
뷰 상에서는 제대로 나오지만 탭바에서 이미지가 불투명한 부분을 틴트컬러로 보이게 한다.

```swift
let image = UIImage(named: "")?.withRenderingMode(.automatic)
```

#### 2. alwaysOriginal

원본 이미지에서 컬러정보가 모두 보이는 것으로 말 그대로 원본의 색상 정보 그대로 이미지에 보여진다.

```swift
let image = UIImage(named: "")?.withRenderingMode(.alwaysOriginal)
```

#### 3. alwaysTemplate

원본 이미지에서 컬러 정보가 모두 제거되고 불투명한 부분을 틴트컬러로 보이게 한다.<br>
즉, 원본 이미지가 가지고 있는 컬러정보는 사라지고 내가 지정한 tintColor로 이미지의 색상이 보여지는 것을 의미한다.

```swift
let button = UIButton()
button.setImage(UIImage(named: "")?.withRenderingMode(.alwaysTemplate), for: .normal)
button.tintColor = UIColor.red
```

이런식으로 코드가 짜여진다면 버튼에 들어가는 이미지의 원본 색상은 사라지고 내가 지정한 tintColor인 red 색상이 버튼 이미지의 색상이 될 것이다. 

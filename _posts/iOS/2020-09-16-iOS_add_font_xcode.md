---
layout: post
title: Xcode에 Custom Font 추가하기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Xcode에 Custom Font 추가하기

xcode에서 기본으로 제공해주는 폰트가 아닌 커스텀 폰트를 적용하고 싶을 때 아래와 같은 방법을 사용하자!


### 1. 프로젝트 파일에 폰트 파일을 import

우선 원하는 폰트 파일을 다운로드한다.<br>
**.ttf .otf** 파일 모두 지원되지만 .wotf 파일은 인식하지 못한다.


<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/55.png" alt="" width="50%">
</figure>
</center>

이때 중요한 것은 import시킬 때, 아래와 같은 창이 생기는데, **Add to targets** 에 자신이 만든 프로젝트를 반드시 체크해야한다는 것이다. target 설정을 하지 않으면 폰트 파일을 프로젝트 내부에서 인식하지 못하기 때문이다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/56.png" alt="" width="50%">
</figure>
</center>

(추가로 폰트 파일은 디렉토리 depth 상관없이 인식시킬 수 있기 때문에 나는  **{프로젝트 디렉토리}/SupportingFiles/Fonts/디렉토리** 에 위치시켰다.


### 2. Info.plist에 폰트파일 정의

Info.plist파일은 프로젝트의 내용을 요약해놓은 파일이기 때문에 우리가 어떤 커스텀 폰트가 사용되는지 정의해줘야 한다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/57.png" alt="" width="50%">
</figure>
</center>

Info.plist파일 내부 > Information Property List > **Fonts provided by application** 항목이 존재한다면 해당 리스트에 새로운 아이템을 추가하고 없다면 Fonts provided by application 항목을 생성해준다.

생성한 Fonts provided by application에 사용한 커스텀 폰트 파일명을 정의해주고, 정의할 때 주의할 점은 **파일명의 확장자** 까지 다 적어줘야 한다는 것이다. (Apple SD Gothic Neo Bold.otf 와같이 다 적어줘야 폰트인식을 한다.)


### 3. 코드에 적용

**@IBOutlet** 를 통해 UIFont를 잡아줘야한다. <br>
잘 모르겠지만 위와 같은 방식으로만 하면 스토리보드상에서 글꼴이 나타나지 않는것 같다. (내가 잘못한 걸수도..)

#### Extensions/UIFont.swift

```swift
import UIKit

extension UIFont {
    class func AppleSDGothic(type: AppleSDGothicType, size: CGFloat) -> UIFont! {
        guard let font = UIFont(name: type.name, size: size) else {
            return nil
        }
        return font
    }

    public enum AppleSDGothicType {
        case Bold

        var name: String {
            switch self {
            case .Bold:
                return "AppleSDGothicNeo-Bold"

            }
        }
    }
}
```

#### ViewControllers/UserCell.swift

```swift
func initUI() {
    self.name.font = UIFont.AppleSDGothic(type: .Bold, size: 16)
    self.contact.font = UIFont.AppleSDGothic(type: .Bold, size: 13)
//        self.contact.textColor = UIColor(red: 134, green: 142, blue: 150, alpha: 0)
    self.regdate.font = UIFont.AppleSDGothic(type: .Bold, size: 13)
}
```

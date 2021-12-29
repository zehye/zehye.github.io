---
layout: post
title: UIKit와 SwiftUI 무엇이 다를까?
category: SwiftUI
tags: [SwiftUI]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
참고 블로그는 아래와 같습니다.            
[](https://www.hohyeonmoon.com/blog/swiftui-data-flow/)

<hr>

## SwiftUI?

- 스위프트 언어로 모든 플랫폼에서 앱에 대한 UI와 동작을 선언해주는 프레임워크 > 선언형 프로그래밍
  - 선언형 프로그래밍: 무엇을 수행하는가에 집중을 한 프로그래밍 기법 > 코드 가독성과 재사용성을 높임
- 접근성 및 지역화 등 다양한 기능을 자동으로 지원해줌 (다크모드, 유동적 글자 크기 조정 등)
- UIKit, AppKit, WatchKit 프레임워크 객체와 통합해 플랫폼 별 더 많은 기능을 활용할 수 있음
- 캔버스와 코드가 동기화되어 MVVM 아키텍쳐 패턴에 적합
- Xcode12, iOS13 이상부터 지원 가능
- 뷰 프로토콜을 채택하고 바디를 구현해주어 뷰를 사용하게함
- 공간의 개념으로 뷰를 만들면 중앙부터 차지하게됨 (상대적이 아님)

### SwiftUI 장점?

- 한번의 개발로 모든 플랫폼에서 동작이 가능한 앱 구현이 가능
- 생명주기 관리 필요가 없음
- 스토리보드를 통한 MVC 아키텍쳐 패턴의 작업 시 데이터의 변화 상태를 바로 볼 수 없는 등의 단점을 해결
- 코드 작성과 동시에 디자인 인터페이스가 생성되고 렌더링이 됨
- .Modifier(.)로 체이닝을 통해 스타일링에 대해 구현이 가능
- 유지보수 시 코드 가독성이 좋고 편리
- 오토레이아웃을 일일히 해주지 않아도됨

### SwiftUI 단점?

- 아직 UIKit을 전부 대체하지 못함
- 낮은 버전에 대해 지원이 되지 않아 Deployment Target이 한정적
- 나온지 얼마 되지 않아 아직 버그 및 문제가 많음
- 라이브러리들의 부실한 지원

### SwiftUI와 UIKit?

- SwiftUI는 UIKit의 대체는 아니다. 이유는 SwiftUI의 많은 기능들이 UIKit 상에서 작동한다.
- UIKit은 이벤트 중심 프레임워크이고 SwiftUI는 상태 중심 프레임워크
- UIKit 위에서 빌드된다는 말로 코드가 내부에서 UIKit에 있는 컴포넌트 소스로 변환해 컴파일 한다는 의미
- 동시에 사용하려면 UIHostingController를 사용!
- UIKit의 경우 view들이 UIView 클래스를 상속받는데, SwiftUI는 구조체이면서 view protocol만 준수하면 됨

### UIKit -> SwiftUI로 변화된 UI 요소

- UITableView: List
- UICollectionView: No SwiftUI equivalent
- UILabel: Text
- UITextField: TextField
- UITextField with isSecureTextEntry set to true: SecureField
- UITextView: No SwiftUI equivalent
- UISwitch: Toggle
- UISlider: Slider
- UIButton: Button
- UINavigationController: NavigationView
- UIAlertController with style .alert: Alert
- UIAlertController with style .actionSheet: ActionSheet
- UIStackView with horizontal axis: HStack
- UIStackView with vertical axis: VStack
- UIImageView: Image
- UISegmentedControl: SegmentedControl
- UIStepper: Stepper
- UIDatePicker: DatePicker
- NSAttributedString: Incompatible with SwiftUI; use Text instead.

#### SwiftUI Property Wrappers?

- **@State**: 상태변화를 감지하도록 사용
  - 일반적으로 struct는 값 타입이여서 struct내의 값을 변경할 수 없다.
  - SwiftUI는 **@State** 를 제공해 struct내의 값을 변경할 수 있게 해준다.
  - SwiftUI의 view는 struct이고, 이는 언제든 소멸되거나 재생성된다.
  - 그렇기 때문에 **@State** 를 사용해 지속적으로 변형 가능한 변수를 만드는 것이다.
  - 단, **@State** 는 String, Int, Bool과 같은 간단한 타입에만 사용되는 것이 좋다.
  - 일반적으로 **@State** 변수는 private으로 선언되고, 다른 view와 공유되지 않는다.
  - 다른 view와 값을 공유하고 싶다면, **@ObservedObject** 나 **@EnvironmentObject** 를 사용하면 된다.

```swift
import SwiftUI

struct ContentView: View {
    @State var userName: String = ""
    var body: some View {
        VStack {
            TextField("Enter your ID", text: $userName)  // $를 통해 state propery를 호출
                .border(Color(UIColor.separator))
            // userName값이 변하면 userName을 참조하는 컴포넌트가 새로 렌더링 되어 값을 표현함
            Text("Welcome, \(userName)!")

        }
        .textFieldStyle(RoundedBorderTextFieldStyle())
        .padding()
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
```

- **@Binding**: 정의된 **@State** 변수를 사용하고 해당 값을 넘겨받을거란 의미로 다른 부분에서 변경이 일어나도 자동 연결 변경됨
  - **@Binding** 은 부모 view의 **@State** 와 같은 값을 양방향으로 연결되도록 해준다.
  - 즉 자신의 뷰가 아닌 부모 뷰에서 값을 가져오고 싶을때 사용하는 키워드 > 뷰 끼리 양방향으로 연결해서 서로 값을 공유할 수 있도록 해주는 키워드
  - 아래 코드에서 userName은 message를 바인딩 시켜줘서 값을 변경해준다.

```swift
import SwiftUI

struct ContentTextView: View {
    @Binding var message: String

    var body: some View {
        Text(message)
            .padding(10)
            .border(Color.orange, width: 1.5)
    }
}

struct ContentView: View {
    @State var userName: String = ""
    var body: some View {
        VStack {
            TextField("Enter your ID", text: $userName)
                .border(Color(UIColor.separator))
            ContentTextView(message: $userName)
        }
        .textFieldStyle(RoundedBorderTextFieldStyle())
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

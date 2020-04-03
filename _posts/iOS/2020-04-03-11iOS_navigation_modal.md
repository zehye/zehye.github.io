---
layout: post
title: 네비게이션과 모달의 차이점은 무엇인가? +구현하는 방식
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.     
[참고한 블로그](https://gwangyonglee.tistory.com/47)

<hr>

## 네비게이션과 모달

이전에 네비게이션 컨트롤러와 뷰 컨트롤러의 차이점에 대한 블로그를 작성한 적이 있다 >> [블로그 참고](https://www.zehye.kr/ios/2020/03/13/iOS_vc_navigation_vc/)<br>
네비게이션과 모달 모두 뷰 컨트롤러를 제어한다는 공통점이 잇지만, 표현방식에서의 차이를 보여준다.

**네비게이션** 은 스택구조로 뷰를 쌓아올려 최상위 뷰만을 보여주는 형태를 말한다. 따라서 뷰를 스택에 담을때 화면에 보여주기 위해서는 push, 화면에서 제거할 때는 pop을 사용한다. **모달** 은 네비게이션과 다르게 새로운 뷰를 보여주는 방식이 아닌 현재 화면을 덮는 형태를 말한다. 화면에 뷰를 보여주기 위해서 present, 화면에서 내릴때는 dismiss를 사용하는 것이 특징이다.


### 용도의 차이

네비게이션은 주로 정보의 깊이와 흐름을 가지는 구조에서 사용된다. 네비게이션 바 타이틀을 통해 현재 위치를 확인할 수 있으며 이전 화면으로 돌아가는 버튼 또한 네비게이션 컨트롤러에서 제공한다. 설정앱을 생각해보면 쉽게 네비게이션이 무엇인지 이해하기 쉬울 것이다. 하나의 정보 흐름을 가지고 깊게 파고드는 형식! [설정] > [Apple ID] > [이름, 전화번호, 이메일]...

반면 모달은 화면 전체를 정보의 흐름에서 벗어나 전환해주는 것을 의미한다. >> [모달이란?](https://www.zehye.kr/ios/2020/01/26/11iOS_modal/)

이전 블로그 내용을 다시 조금 정리해보자면, 모달은 사용자의 이목을 끌기위해 사용하는 화면 전환 기법을 의미한다. 이목을 집중해야하는 화면을 다른 화면 위로 띄워(present) 표현하는 방식으로 모달로 보이는 화면을 사라지게 하려면 반드시 특정 선택을 해야하는 특징을 가진다. 위에서 언급했던 것과 같이 모달은 네비게이션 인터페이스와는 달리 정보의 흐름을 가지고 화면을 이동한다기 보다는 꼭 이목을 끌어야 하는 화면에서 사용하는것이 특징이다. 되도록 단순하고 사용자가 빠르게 처리할 수 있는 내용을 표현하는 것이 좋다. [정보의 흐름이 아닌 잠깐의 팜업 혹은 잠시간의 입력폼 >> 아이폰 내의 구글 메일앱]


### 네비게이션 구현 방식

```swift
@IBAction func signUpBtn(_ sender: UIButton) {
    let storyboard = UIStoryboard.init(name:"Main", bundle: nil)
    let viewController: SecondViewController = storyboard.instantiateViewController(withIdentifier: "SecondViewController") as! SecondViewController
    self.navigationController?.pushViewController(viewController, animated: true)
}
```

### 모달 구현 방식

```swift
let storyboard: UIStoryboard = UIStoryboard(name: "SecondStoryboard", bundle: nil)
if let myViewController: MyViewController = storyboard.instantiateViewController(withIdentifier: "MyViewController") as? MyViewController {
	self.present(myViewController, animated: true, completion: nil)
}
```


구현하는 방식은 각자 조금의 차이는 있겠지만, 기본적으로 나는 위의 형식으로 구현한다.

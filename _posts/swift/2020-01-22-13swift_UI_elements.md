---
layout: post
title: swift UIButton, UISlider, UILabel
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## UIButton, UISlider, UILabel

애플리케이션 화면을 구현할 때 자주 사용하는 UI요소들이다.



## UIButton

사용자의 상호작용(터치/탭 등의 이벤트)에 반응해 미리 지정된 코드를 실행하는 컨트롤 요소


**버튼 생성의 3단계**<br>
1. 버튼을 생성하고 버튼의 유형을 선택
2. 버튼을 나타내기 위한 문자(타이틀)을 입력하거나, 이미지를 설정한 뒤 크기를 조정
3. 버튼에 특정 이벤트가 발생할 때 작동할 하나 이상의 메서드를 연결


**버튼과 메서드 연결하는 방법**<br>
1. `addTarget(_:action:for:)` 메서드 사용
2. 인터페이스 빌더에서 연결 `(@IBAction)`


이때 버튼과 연결되는 메서드 형식은 아래와 같다.

```swift
func doSomething()
func doSomething(sender: UIButton)
func doSomething(sender: UIButton, forEvent event: UIEvent)
```


**버튼의 상태**: default, highlighted, focused, selected, disabled

버튼의 상태는 조합된 상태일 수 있다. 예) [default + highlighted], [selected + disabled] 등등<br>
버튼 생성 시 기본 상태 값은 `default`이며, 사용자가 버튼과 상호작용을 하면 상태 값이 변하게 된다.

그리고, 프로그래밍 방식 혹은 인터페이스 빌더를 이용해 버튼의 각 상태에 대한 속성을 별도로 지정할 수 있습니다. 만약 별도로 속성을 지정하지 않으면 UIButton 클래스에서 제공하는 기본 동작을 사용하게 된다. 예) disabled 버튼은 일반적으로 흐리게 표시되며 사용자가 탭 해도 highlighted 되지 않는다.

버튼의 프로퍼티 값을 설정하는 방식에는 코드를 이용하는 방법과 스토리보드의 인스팩터를 이용하는 방법이 있다.


#### enum UIButtonType: 버튼의 유형

버튼의 유형에 따라 버튼의 기본적인 외형과 동작이 달라진다. 처음 버튼을 생성할 때

1. init(type:) 메서드를 이용 혹은
2. 인터페이스빌더의 "Attribute Inspector"에서 버튼 유형을 지정할 수 있다

이때 한번 생성된 버튼의 유형은 이후 변경 할 수 없으며, 가장 많이 사용하는 유형은 `Custom`과 `System`이지만 필요에 따라 다른 유형(Detail Disclosure, Info Light, Info Dark, Add Contact)를 사용할 수 있다.

- var titleLabel: UILabel?: 버튼 타이틀 레이블
- var imageView: UIImageView?: 버튼의 이미지 뷰
- var tintColor: UIColor!: 버튼 타이틀과 이미지의 틴트 컬러


#### 버튼의 주요 메서드

```swift
// 특정 상태의 버튼의 문자열 설정
func setTitle(String?, for: UIControlState)

// 특정 상태의 버튼의 문자열 반환
func title(for: UIControlState) -> String?

// 특정 상태의 버튼 이미지 설정
func setImage(UIImage?, for: UIControlState)

// 특정 상태의 버튼 이미지 반환
func image(for: UIControlState) -> UIImage?

// 특정 상태의 백그라운드 이미지 설정
func setBackgroundImage(UIImage?, for: UIControlState)

// 특정 상태의 백그라운드 이미지 반환
func backgroundImage(for: UIControlState) -> UIImage?

// 특정 상태의 문자열 색상 설정
func setTitleColor(UIColor?, for: UIControlState)

// 특정 상태의 attributed 문자열 설정
func setAttributedTitle(NSAttributedString?, for: UIControlState)
```


## UILabel

한줄 또는 여러줄의 텍스트를 보여주는 뷰로, UIButton등의 컨트롤의 목적을 설명하기 위해 사용하는 경우가 많다.


**레이블 생성의 3단계**<br>
1. 레이블을 생성
2. 레이블이 표시할 문자열을 제공
3. 레이블의 모양 및 특성을 설정


#### 레이블 주요 프로퍼티

```swift
var text: String? // 레이블이 표시할 문자열, 문자열이 모두 동일한 속성(폰트, 색상, 기울임꼴 등)으로 표시됩니다.
// text 프로퍼티에 값을 할당하면 attributedText 프로퍼티에도 똑같은 내용의 문자열이 할당됩니다.

var attributedText: NSAttributedString? :  // 레이블이 표시할 속성 문자열
// NSAttributed 클래스를 사용한 속성 문자열 중 특정 부분의 속성을 변경할 수 있다([예] 일부 글자 색상 변경/일부 글자 폰트 변경)
// attributedText 프로퍼티에 값을 할당하면 text 프로퍼티에도 똑같은 내용의 문자열이 할당

var textColor: UIColor! :  // 문자 색상
var font: UIFont!:  // 문자 폰트

var textAlignment: NSTextAlignment:  // 문자열의 가로 정렬 방식으로 left, right, center, justified, natural 중 하나를 선택

var numberOfLines: Int:  // 문자를 나타내는 최대 라인 수
// 문자열을 모두 표시하는 데 필요한 만큼 행을 사용하려면 0으로 설정, 기본 값은 1
// 설정한 문자열이 최대 라인 수를 초과하면 lineBreakMode 프로퍼티의 값에 따라 적절히 잘라서 표현
adjustsFontSizeToFitWidth  // 프로퍼티를 활용하면 폰트 사이즈를 레이블의 넓이에 따라 자동으로 조절해줌

var baselineAdjustment: UIBaselineAdjustment  // 문자열이 Autoshrink 되었을 때의 수직 정렬 방식

Align Baseline  // 문자가 작아졌을 때 기존 문자열의 기준선에 맞춤
Align Center  // 문자가 작아졌을 때 작아진 문자의 중앙선에 맞춤
None  // 문자가 작아졌을 때 기존 문자열의 위쪽 선에 맞춤

var lineBreakMode: NSLineBreakMode  // 레이블의 경계선을 벗어나는 문자열에 대응하는 방식

Character wrap  // 여러 줄 레이블에 주로 적용되며, 글자 단위로 줄 바꿈을 결정
Word wrap  // 여러 줄 레이블에 주로 적용되며, 단어 단위로 줄 바꿈을 결정

Truncate head  // 한 줄 레이블에 주로 적용되며, 앞쪽 텍스트를 자르고 ...으로 표시
Truncate middle  // 한 줄 레이블에 주로 적용되며, 중간 텍스트를 자르고 ...으로 표시
Truncate tail  // 한 줄 레이블에 주로 적용되며, 끝쪽 텍스트를 자르고 ...으로 표시, 기본 설정 값임
```


## UISlider

연속된 값 중에서 특정 값을 선택하는 데 사용되는 컨트롤


**슬라이더 생성의 3단계**<br>
1. 슬라이더를 생성하고, 슬라이더가 나타내는 값의 범위를 지정
2. 적절한 색상과 이미지를 이용해 슬라이더의 모양을 구성
3. 하나 이상의 메서드를 슬라이더와 연결


**사용자 상호작용에 반응하기**<br>
사용자가 슬라이더의 값을 변경하면 슬라이더에 연결된 메서드가 호출되어 원하는 작업이 실행<br>
기본적으로는 사용자가 슬라이더의 `Thumb`를 이동시키면 연속적으로 이벤트를 호출하지만, isContinous 프로퍼티값을 false로 설정하면 슬라이더의 Thumb에서 손을 떼는 동시에 이벤트를 호출

**슬라이더와 메서드 연결하는 방법**<br>
- `addTarget(_:action:for:)` 메서드 사용
- 인터페이스 빌더에서 연결 `(@IBAction)`


#### 슬라이더와 연결하는 메서드 형식

슬라이더의 값을 변경했을 때 필요한 정보에 따라 아래 세 가지 중 한 가지를 선택하여 사용


```swift
func doSomething()
func doSomething(sender: UISlider)
func doSomething(sender: UISlider, forEvent event: UIEvent)
```

**슬라이더 주요 프로퍼티**

슬라이더의 프로퍼티 값을 설정하는 방식에는 프로그래밍 방식과, 스토리보드의 인스펙터를 이용한 방법이 있다.

- var minimumValue: Float, var maximumValue: Float: 슬라이더 양끝단의 값
- var value: Float: 슬라이더의 현재 값
- var isContinuous: Bool: 슬라이더의 연속적인 값 변화에 따라 이벤트 역시 연속적으로 호출할 것인지의 여부
- var minimumValueImage: UIImage?, var maximumValueImage: UIImage?: 슬라이더 양끝단의 이미지
- var thumbTintColor: UIColor?: thumb의 틴트 색상
- var minimumTrackTintColor: UIColor?, var maximumTrackTintColor: UIColor?: thumb를 기준으로 앞쪽 트랙과 뒤쪽 트랙의 틴트 색상


#### 슬라이더 주요 메서드

```swift
//  슬라이더의 현재 값 설정
func setValue(Float, animated: Bool)

//  특정 상태의 minimumTrackImage 반환
func minimumTrackImage(for: UIControlState) -> UIImage?

// 특정 상태의 minimumTrackImage 설정
func setMinimumTrackImage(UIImage?, for: UIControlState)

// 특정 상태의 maximumTrackImage 반환
func maximumTrackImage(for: UIControlState) -> UIImage?

// 특정 상태의 minimumTrackImage 설정
func setMaximumTrackImage(UIImage?, for: UIControlState)

//  특정 상태의 thumbImage 반환
func thumbImage(for: UIControlState) -> UIImage?

//특정 상태의 thumbImage 설정
func setThumbImage(UIImage?, for: UIControlState)
```

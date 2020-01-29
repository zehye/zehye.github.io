---
layout: post
title: Cocoa Touch 프레임워크와 UIKit, Foundation
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Cocoa Touch 프레임워크란?

iOS 애플리케이션 개발 환경으로, 애플리케이션의 다양한 기능 구현에 필요한 여러 프레임워크를 포함하는 최상위 레벨의 프레임 워크

- macOS 애플리케이션 제작에 사용하는 프레임워크
- 핵심 프레임워크인 `UIKit`와 `Foundation`을 포함


### UIKit 프레임워크란?

iOS 애플리케이션의 사용자 인터페이스를 구현하고 이벤트를 관리하는 프레임워크

- 제스처처리, 애니메이션, 그림 그리기, 이미지 처리, 텍스트 처리 등 사용자 이벤트 처리를 위한 클래스를 포함
- 테이블 뷰, 슬라이더, 버튼, 텍스트필드, 얼럿 창 등 애플리케이션의 화면을 구성하는 요소를 포함
- UIKit 클래스 중 UIResponder에서 파생된 클래스나 사용자 인터페이스 관련된 클래스는 애플리케이션 메인 스래드에서만 사용
- UIKit는 iOS와 tvOS플랫폼에서 사용

### UIKit 기능별 요소

#### 사용자 인터페이스

- View and Control: 화면에 콘텐츠 표시
- View Controller: 사용자 인터페이스 관리
- Animation and Haptics: 애니메이션과 햅틱을 통한 피드백 제공
- Window and Screen: 뷰 계층을 위한 윈도우 제공


#### 사용자 액션

- Touch, Press, Gesture: 제스처 인식기를 통한 이벤트 처리 로직
- Drag and Drop: 화면 위에서 드래그 앤 드롭 기능
- Peek and Pop: 3D 터치에 대응한 미리 보기 기능
- Keyboard and Menu: 키보드 입력을 처리 및 사용자 정의 메뉴 표기

> 생각해보기 새롭게 ViewController를 생성하면 상단에 'import UIKit'이 기본적으로 명시되어있는걸 본적 있다 <br>
왜 ViewController와 UIKit는 단짝일까요?

**내가 생각한 답** <br>
기본적으로 UIKit는 iOS 사용자 인터페이스를 구현하는 프레임워크이다. 사용자 인터페이스를 관리하는 View Controller를 건들기 위해서는 기본적으로 이를 관리하는 프레임워크인 UIKit가 import되어야 한다고 생각한다.



### Foundation

원시 데이터타입(String, Int, Double), 컬렉션 타입(Array, Dictionary, Set) 및 운영체제 서비스를 사용해 애플리케이션의 기본적인 기능을 관리하는 프레임워크

- 데이터타입, 날짜 및 시간 계산, 필터 및 정렬, 네트워킹 등의 기본 기능 제공
- 프레임워크에서 정의한 클래스, 프로토콜 및 데이터 타입은 iOS뿐 아니라 macOS, watchOS, tvOS등 모든 애플 SDK에서 사용

Foundation에서 제공하는 데이터타입 및 컬렉션 타입의 대부분은 Objective-C 언어의 기능에서 지원하지 않는 것이기 때문에 언어기능을 보완하기 위한 구현이며, Swift에서는 이에 해당하는 데이터 타입과 기능 대부분을 [Swift표준 라이브러리](https://developer.apple.com/documentation/swift)에서 제공한다.


### Foundation 기능별 요소

#### 기본

- Number, Data, String: 원시 데이터 타입 사용
- Collection: Array, Dictionary, Set등과 같은 컬렉션 타입 사용
- Data and Time: 날짜와 시간을 계산하거나 비교하는 작업
- Unit and Measurement: 물리적 차원을 숫자로 표현 및 관련 단위 간 변환 기능
- Data Formmating: 숫자, 날짜, 측정값 등을 문자열로 변환 또는 반대 작업
- Filter and Sorting: 컬렉션의 요소를 검사하거나 정렬하는 작업

#### 애플리케이션 지원

- File System: 파일 또는 폴더를 생성하고 읽고 쓰는 기능 관리
- Archives and Serialization: 속성목록, JSON, 바이너리 파일들을 객체로 변환 또는 반대작업 관리
- iCloud: 사용자의 iCloud 계정을 이용해 데이터를 동기화하는 작업 관리

#### 네트워킹

- URL Loading System: 표준 인터넷 프로토콜을 통해 URL과 상호작용하고 서버와 통신하는 작업
- Bonjour: 로컬 네트워크를 위한 작업


> 생각해보기.
새롭게 ViewController 파일을 생성하면 상단에 'import UIKit'이 기본적으로 명시되어있다.
그렇다면 어떤 파일을 생성하면 'import Foundation'이 기본적으로 명시되어있을까요?


**내가 생각한 답** <br>
원시 데이터 타입 및 컬렉션 타입을 사용하기 위해 기본적인 Swift파일을 생성할때 import Foundation이 된다. UIKit 내부에도 Foundation이 들어있지만, 이를 분리해서 import를 하는 이유는 iOS의 디자인 패턴인 MVC 모델을 생성할 때 사용자 인터페이스와 비즈니스 로직을 분리하기 위해 데이터 타입에서 쓰이는 Foundation을 쓰도록 권장한 것이 아닐까 싶다.

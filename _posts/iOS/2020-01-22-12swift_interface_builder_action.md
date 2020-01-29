---
layout: post
title: swift 인터페이스 빌더와 객체를 코드와 연결(IBAction)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 인터페이스 빌더와 객체를 코드와 연결(IBAction)

인터페이스 빌더에서 생성된 객체에서 발생한 액션을 코드와 연결한다. > 사용자 동작에 상호 작용을 할 수 있도록 한다

즉, 실제 앱에서 터치를 통해 일어나는 반응을 확인하는 작업!

### 컨트롤 이벤트의 종류

컨트롤 이벤트는 UIControl에 Event라는 타입으로 정의되어 있다. 아래는 컨트롤 객체에 발생할 수 있는 이벤트의 종류!

#### touchDown

컨트롤을 터치했을 때 발생하는 이벤트 > UIControl.Event.touchDown

#### touchDownRepeat

컨트롤을 연속 터치 할 때 발생하는 이벤트 > UIControl.Event.touchDownRepeat

#### touchDragInside

컨트롤 범위 내에서 터치한 영역을 드래그 할 때 발생하는 이벤트 > UIControl.Event.touchDragInside

#### touchDragOutside

터치 영역이 컨트롤의 바깥쪽에서 드래그 할 때 발생하는 이벤트 > UIControl.Event.touchDragOutside

#### touchDragEnter

터치 영역이 컨트롤의 일정 영역 바깥쪽으로 나갔다가 다시 들어왔을 때 발생하는 이벤트 > UIControl.Event.touchDragEnter

#### touchDragExit

터치 영역이 컨트롤의 일정 영역 바깥쪽으로 나갔을 때 발생하는 이벤트 > UIControl.Event.touchDragExit

#### touchUpInside

컨트롤 영역 안쪽에서 터치 후 뗐을때 발생하는 이벤트 > UIControl.Event.touchUpInside

#### touchUpOutside

컨트롤 영역 안쪽에서 터치 후 컨트롤 밖에서 뗐을때 이벤트 > UIControl.Event.touchUpOutside

#### touchCancel

터치를 취소하는 이벤트 (touchUp 이벤트가 발생되지 않음) > UIControl.Event.touchCancel

#### valueChanged

터치를 드래그 및 다른 방법으로 조작하여 값이 변경되었을때 발생하는 이벤트 > UIControl.Event.valueChanged

#### primaryActionTriggered

버튼이 눌릴때 발생하는 이벤트 (iOS보다는 tvOS에서 사용) > UIControl.Event.primaryActionTriggered

#### editingDidBegin

UITextField에서 편집이 시작될 때 호출되는 이벤트 > UIControl.Event.editingDidBegin

#### editingChanged

UITextField에서 값이 바뀔 때마다 호출되는 이벤트 > UIControl.Event.editingChanged

#### editingDidEnd

UITextField에서 외부객체와의 상호작용으로 인해 편집이 종료되었을 때 발생하는 이벤트 > UIControl.Event.editingDidEnd

#### editingDidEndOnExit

UITextField의 편집상태에서 키보드의 return 키를 터치했을 때 발생하는 이벤트 > UIControl.Event.editingDidEndOnExit

#### allTouchEvents

모든 터치 이벤트 > UIControl.Event.allTouchEvents

#### allEditingEvents

UITextField에서 편집작업의 이벤트 > UIControl.Event.allEditingEvents

#### applicationReserved

각각의 애플리케이션에서 프로그래머가 임의로 지정할 수 있는 이벤트 값의 범위 > UIControl.Event.applicationReserved

#### systemReserved

프레임워크 내에서 사용하는 예약된 이벤트 값의 범위 > UIControl.Event.systemReserved

#### allEvents

시스템 이벤트를 포함한 모든 이벤트 > UIControl.Event.allEvents

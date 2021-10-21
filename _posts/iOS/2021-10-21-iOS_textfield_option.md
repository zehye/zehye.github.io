---
layout: post
title: iOS UITextField 각종 옵션들 정리
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## UITextField

### ClearButton(내용 삭제)

- never: 모든 경우 보이지 않음
- while editing: 입력 중인 경우 버튼이 보임
- unless editing: 입력이 완료되어야 버튼이 보임
- always: 모든 경우에 버튼이 보임


### Adjust to Fit

텍스트필드의 크기가 줄어들 때 텍스트의 크기도 줄여야할지를 지정해 줌

- min Font Size: 폰트 최소 크기를 지정


### Text Input Traits

- Capitalization: 첫 글자를 자동으로 대문자화
- Keyboard Type
  - Number Pad: 숫자만 입력 > Return Key 없음
- Return Key
  - Auto-enable Return Key: 글자가 없을 경우 리턴 키 비활성화 하고 싶다면 사용 > true
- Secure Text Entry: 비밀 번호의 경우 true 로 지정



### Autoresize Subviews

뷰의 크기가 변할 때 하위 뷰의 크기를 변경하게 해줄지를 지정해 줌(default: false)

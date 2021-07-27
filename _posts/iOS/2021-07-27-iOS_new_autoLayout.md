---
layout: post
title: 오토레이아웃을 더 공부하며 알게 된 점 정리해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
공부하면서 더 추가해나갈 예정입니다. 틀린 부분 혹은 잘못된 부분이 있다면 알려주세요! :) 

<hr>

## AutoLayout

view의 절대적인 좌표가 아닌 상대적인 좌표를 요할 때 사용한다.<br>
오토레이아웃이 없으면 view의 크기는 고유 크기를 지니고 있다. (width, height)


### multiplier 

- 연결하는 방식에 따라 비율이 달라진다.
- 1>2, 2>1로 연결하냐에 따라 First Item, Second Item 이 정해지고 비율이 설정된다.

**alignmebt + multiplier**

1. 부모뷰를 기준으로 이동
2. 하나만 선택했을때 가로, 세로 정렬을 선택할 수 있음
  - Horizontally in Container, Vertically in Container 설정
3. Align CenterX: 0.1 > 왼쪽으로 이동, 1.5 > 오른쪽으로 이동
4. Align CenterY: 0.1 > 위로 이동, 1.5 > 아래로 이동


### View를 겹치게 하기 

2개 이상의 겹쳐진 뷰를 만들 때 단순히 자식뷰의 multiplier 값을 변경하는 것으로 한다면 부모 view가 어떤 모양을 가지거나 라운드 처리를 해야하는 등의 view라면 <br>
inspector에서 clip to bounds 속성을 체크하게 된다. 이러면 부모뷰에서 겹쳐지지 않은 영역은 잘리게 된다. > **동등하지 않은 레벨의 정렬**

view를 겹쳐지게 하기 위해서 자식 뷰를 부모 뷰에서 꺼내고 두 뷰를 선택한 뒤 Top Edges와 Horizontal Centers를 체크한다. > **동등한 레벨의 정렬**

그리고 겹쳐질 뷰의 Align Center Y의 Second Item을 Center Y 로 변경한다.


### Priority

priority는 우선 Required(1000), High(750), Low(250)이 기본이며 원하는 만큼의 값을 직접 입력할 수 있다.<br>
일단 기본으로 1000priority가 주어지는데 이것은 강한 우선순위로 다른 우선순위에 영향을 준다.

- Hugging: 내 컨텐츠에 대한 본연의 사이즈 유지 우선순위
- Compression Resistance: 배치한 방향에 대한 우선순위 (작지않게 버티겠다)
  - 우선순위가 크면 내 크기를 유지하고
  - 우선순위가 작으면 내 크기를 작게한다
  - 숫자가 높을수록 우선순위가 높고
  - 낮은 우선 순위는 늘어나게 된다.


### Portrait(세로), Landscape(가로)

- constraints는 세로모드일때와 같이 동일하게 지정
- Vary for Traits 메뉴를 통해 width/height 변경에 따라 적용 가능한 디바이스 확인 가능
- Vary for Traits에서 가로/세로 모드일때 적용하고 싶은 대로 뷰를 다시 정의함


### stretching 

- x,y,w,h에 대한 설정 가능
- 값이 0일때는 시작점붜 늘어나게 하는 것(제약없이 이미지가 늘어남, 단순 이미지 확대용)
  - width를 0.5로 한 경우 시작점부터 50% 영역까지 늘어남
  - y를 0.5로 한 경우 50%영역 이상 늘어남




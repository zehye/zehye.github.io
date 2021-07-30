---
layout: post
title: UIView의 ContentMode 정리
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## ContentMode

UIImageView안에 이미지를 삽입하려하다보면 각각의 이미지를 어떤 비율로 넣을 지 정해주어야 하는데, <br>
이때 UIView에서 ContentMode로 정해줄 수 있다.

기본적으로 top, bottom, left, right 등이 존재하지만 이 속성들은 직관적으로 바로 느껴질 것이다. > 위, 아래, 왼쪽, 오른쪽<br>
외에 많이 사용하지만 헷갈리는 `scaleToFill, scaleAspectFit, scaleAspectFill` 이 세가지를 정리해보자.


### scaleToFill

콘텐츠의 비율을 변경하여 View 크기에 맞게 확장하는 옵션



### scaleAspectFit

콘텐츠의 비율을 유지하며 View의 크기에 맞게 확장하는 옵션으로 남는 영역은 투명하다



### scaleAspectFill

콘텐츠의 비율을 유지하며 View의 크기에 빈 영역 없이 확장하는 옵션으로 일부 내용이 사라질 가능성이 있다.


- aspect: 비율 유지
- fill: 화면에 빈틈없이 꽉차게
- fit: 화면에 딱 맞게

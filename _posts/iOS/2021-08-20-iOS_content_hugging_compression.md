---
layout: post
title: iOS Content Hugging, Content Compression 설정
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

## 개념

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/19.png" alt="" width="50%">
</figure>
</center>

- Content Hugging Priority: 고유 컨텐츠를 둘러싼 컨텐츠를 축소해야하는 저항에서 사용
- Content Compression resistance Priority: 고유 컨텐츠 크기 이상으로 축소해야하는 저항에서 사용


### Content Hugging

- 공간이 남을 때 어떤것을 줄이고 늘릴지 선택할 때 사용 > 주로 스택뷰에서 사용
- hugging의 default 값은 250



### Compression resistance

- 공간이 부족할 때 어떤것을 줄이고 늘릴지 선택할 때 사용
- compression의 default 값은 250

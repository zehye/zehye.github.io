---
layout: post
title: iOS OS_ACTIVITY_MODE 설정하기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## OS_ACTIVITY_MODE

Xcode 버전업을 하고나서부터 불필요한 로그들이 콘솔창에 찍히게 되었다. <br>
이는 Xcode8 버전부터 `OS_ACTIVITY_MODE`가 생겨서인데...

이는 os 시스템 관련 로그를 출력해주는 것으로 정작 너무 많은 로그들이 출력되어서 내 콘솔에 내가 원하는 내 로그가 보이지않게된다는 단점이 존재한다. 나또한 저러한 로그가 정말 한 몇십줄을 가득채워서.. 이를 해결할 수 있는 방법을 찾아보았다.

해결방법은 아래와 같다.

1. Xcode 상단 `Product` 탭 선택
2. Product에서 `Scheme` > `edit scheme`
3. 환경변수(Environment Variables) +버튼 선택
4. Name: OS_ACTIVITY_MODE, value: disable 입력

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/23.png" alt="" width="50%">
</figure>
</center>


이렇게 하면 더이상 콘솔에 시스템 로그는 출력되지 않을것이다.

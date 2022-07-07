---
layout: post
title: iOS xcode multiple commands produce 해결방법 
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## xcode multiple commands produce

프로젝트의 파일명 중에 중복으로 들어갔을 때 이 에러를 마주할 수 있다.<br>
대부분 중복으로 들어가거나 파일에 무언가 문제가 있을때 발생한다.

해결방법은 아래와 같다.

**Project Target > Build Phases > Copy Bundle Resources** 에서 오류난 파일을 (-) 삭제해주면 된다. 

이 방법으로도 해결이 안된다면 **DerivedData** 파일을 삭제해보자.
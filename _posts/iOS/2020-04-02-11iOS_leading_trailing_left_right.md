---
layout: post
title: leading, trailing과 left, right 무엇을 사용하는게 더 좋을까?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.   

<hr>

## leading, trailing과 left, right 무엇을 사용하는게 더 좋을까?

결론부터 이야기하자면, leading, trailig을 이용하는 것을 더 권장한다.

오토레이아웃을 사용하다보면 우리는 leading, trailing, left, right 개념을 알게될텐데 기본적으로 텍스트를 왼쪽에서 오른쪽으로 읽는 우리에게는 leading을 쓰든 left를 쓰든 별 차이가 없을거라고 생각한다. 우리는 당연하게 글을 왼쪽에서부터 읽으니 텍스트의 시작점을 left로 지정을 해도되지만 만약 우리가 만드는 애플리케이션이 타 국가에도 사용되게끔 만드는데 그때에도 left를 사용하게 된다면 문제가 생기게 될 것이다. 그때 우리가 사용해야하는 것이 leading과 trailing이다.

<center>
<figure>
<img src="/assets/post-img/swift/30.png" alt="" width="50%">
</figure>
</center>

- leading: 텍스트의 시작점
- trailing: 텍스트의 끝
- left: 왼쪽
- right: 오른쪽


따라서 애플리케이션의 지역화를 지원해야하는 경우 leading과 trailing은 기본적으로 사용되어야하며, 이런 저런 이유를 떠나서 left, right 딱 왼쪽과 오른쪽을 구분지어 사용하기 보다는 leading, trailing을 사용하여 조금 더 유연하게 코드를 짜는게 옳다고 생각한다. >> LTR과 RTL을 모두 원활히 지원하는 코드로 !!

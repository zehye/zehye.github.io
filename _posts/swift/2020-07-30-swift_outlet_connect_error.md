---
layout: post
title: swift - Outlets cannot be connected to repeating content. 에러가 뜬다면?
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## 문제 상황

> Outlet from the TableViewController to the UILabel is invalid. Outlets cannot be connected to repeating content.

주로 CustomCell을 만들때 발생하는 에러이다.<br>
해당 아울렛을 내가 만들어 준 커스텀셀과 연결되지 않았다는 것을 의미하는 것으로 스토리보드에서 각각 셀을 지정해주면 된다.

<center>
<figure>
<img src="/assets/post-img/swift/54.png" alt="" width="80%">
</figure>
</center>

[Identity Inspector] > [Custom Class] > [Class 설정]

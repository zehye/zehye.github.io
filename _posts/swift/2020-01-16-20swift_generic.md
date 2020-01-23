---
layout: post
title: swift 기본문법 - 제네릭(Generics)
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
[참고1](http://blog.naver.com/PostView.nhn?blogId=jdub7138&logNo=220932097678&categoryNo=109&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=search)
[참고2](https://atelier-chez-moi.tistory.com/80)

<hr>

## 제네릭(Generics)

제네릭은 자료형에 구애받지 않는 코드를 의미한다. 스위프트는 태생적으로 강한 타입의 언어이기 때문에 변수와 인자, 리턴값 등 모두 자료형의 구속을 받게 된다. 이것때문에 때로는 비생산적이고 반복적인 코딩을 해야할 때가 발생하는데 제네릭은 이 부분을 해결해주는 유용한 코드로서 스위프트의 가장 강력한 기능 중 하나이다.

사실 스위프트의 array이나 dictionary도 제네릭이다. 예로 array에는 Int들을 담을 수도 있고 String도 담을 수 있기 때문이다.

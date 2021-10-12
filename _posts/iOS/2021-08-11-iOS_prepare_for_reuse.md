---
layout: post
title: iOS prepareForReuse 란?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## prepareForReuse

테이블 뷰를 사용할때 보통 셀을 재사용하는 경우가 대부분일 것이다. <br>
셀을 말그대로 재사용하기 때문에 재사용된 셀에서 보여주지 않아야 하는 텍스트 혹은 버튼 등이 보여지는 경우가 있다.

말 그대로 재사용했기 때문이다.

예로 들어 생각해보자.

테이블 뷰가 있고 해당 화면에서는 하나의 재사용 셀들이 주르륵 있다. <br>
현재 보여지는 화면으로부터 스크롤을 해서 아래의 셀들이 보여지고 그 셀들 또한 모두 재사용이 되고 있다.<br>
셀은 재사용이 되었지만, 셀 안에 들어가는 데이터의 조건은 각각 다를 수 있다.<br>
그러나 셀은 재사용이 되었기 때문에 원치 않는 정보가 들어가질 수 있다.

셀 그 자체는 안의 데이터가 어떤것이라는것과는 무관하게 <br>
셀 안에 레이블이 들어간다면 그 레이블을 띄워주는 그 자체의 행위만 하기 때문이다.

그렇기 때문에 이렇듯 재사용 셀을 사용할 때는 반드시 모든 값이 초기화 되어져야 한다.<br>
그리고 이렇게 초기화를 하기 위해 호출하는 함수가 `prepareForReuse`이다.
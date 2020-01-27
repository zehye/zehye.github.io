---
layout: post
title: swift 기본문법 - 접근 제한(Access Control)
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>


## 접근 제한(Access Control)

접근권한을 말한다.<br>
class, struct, enum등의 type들을 비롯해 property. method, subscript 등 모두에게 접근제한을 부여할 수 있다.

이를 이해하기 위한 간단한 개념부터 봐보자.


#### Module

특정 기능을 하는 어플리케이션이나 프레임워크의 배포 단위로서 그냥 하나의 프로젝트와 연관된 모든 파일이라고 이해하면 된다. 예로 들어 가계부 앱을 만들었다고 하면, 이 가계부 앱과 관련된 XCode에서 작업하고 배포한 코드 파일 모두를 뜻한다.


#### Source File

프로젝트 파일 하나하나를 의미한다. 보통 한 Source File안에 하나의 class나 하나의 struct를 만드는 것이 일반적이지만 꼭 그럴필요는 없다.


#### Hiding Implementation Detail

어떤 함수나 class 인스턴스 등의 안에서 실행되는 코드를 외부에서는 알 수 없게 가린다는 의미이다. 이에 따라 밖에서는 안에 어떤 property나 method가 있는 지도 알 수 없으며, 이름을 알더라도 접근할 수 없게 만든다.


### 5가지의 Access Level

Swift에는 open, public, internal, fileprivate, private 5가지의 Access Level을 제공한다. 가장 위를 access level이 높다 또는 제한이 가장 없다고 하고, 아래로 갈수록 access level이 낮고 제한이 강하다고 표현한다. 주로 사용되는 것은 `public, private` 두개이다. internal은 모든 객체들이 특별히 access level을 지정해주지 않을 경우 가지는 default access level로서 작업하는 모듈 내에서만 접근 가능하도록 설정되어 있다.


#### open

가장 열린 Access Level. 외부의 다른 모듈이 import 해서 접근 가능하다. 심지어 import하는 외부 모듈안에서 subclass와 override도 가능하기 때문에 위험할 수도 있다. Open API 및 framework 구성시 활용한다.


#### public

open과 마찬가지로 외부 모듈이 import해서 접근 가능하지만, subclass와 override는 오로지 자신의 모듈 안에서만 가능하다. 이 또한 open API 및 framework 구성시 활용한다.


#### internal

자신의 모듈 안에서만 접근 가능하고 외부 모듈에서는 접근 불가하다. 한마디로 프로젝트 내부용이다. 모든 객체는 별도의 지정이 없으면 기본적으로 internal로 셋팅되며 Open API를 제공하지 않는 앱은 대체로 객체들을 internal로 두면 문제될 것이 없다. 다만, 다른 Source file이나 declaration 밖에서의 접근이 없다면 internal로 놔두는 것보다 private으로 두는 것이 더좋다.


#### fileprivate

같은 Source file안에서만 접근 가능하다. 즉, .swift라는 파일 안에서 fileprivate인 함수가 있다면 해당 swift파일안에서는 접근이 가능하지만, 같은 프로젝트의 다른 swift 파일에서는 접근이 불가능하다.


#### private

가장 폐쇄된 Access Level. 같은 declaration 공간({})안에서만 접근가능하다. 즉, 함수 안에서 private으로 선언된 변수는 해당 함수안에서, class안에서 private으로 선언된 변수는 해당 class 안에서만 접근이 가능하다.  

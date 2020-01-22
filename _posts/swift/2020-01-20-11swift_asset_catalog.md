---
layout: post
title: swift 에셋 카탈로그 그리고 앱 시닝과 앱 슬라이싱
category: swift
tags: [swift]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 에셋 카탈로그

Xcode에서 프로젝트를 처음 생성하면 `Asstes.xcassets` 이라는 폴더가 자동으로 생성되는데, 이 폴더에서 애플리케이션에 사용될 다양한 에셋을 관리하며, 이를 에셋 카탈로그라고 한다.

에셋 카탈로그는 에셋과 다양한 디바이스의 속성에 대한 파일의 연결을(mapping) 통해서 애플리케이션 리소스에 쉽게 접근할 수 있도록 도와준다. (리소스는 애플리케이션이 실행 중일 때 사용하는 이미지와 음악 파일 등을 말한다) 여기서 말하는 속성은 디바이스의 특징, 사이즈 클래스, 주문형 리소스, 특정 타입의 정보를 포함하고 있다.

<center>
<figure>
<img src="/assets/post-img/swift/25.png" alt="" width="50%">
</figure>
</center>


- Groups : 그룹은 한 개 이상의 또 다른 그룹이나 에셋을 가질 수 있다
- Assets : 에셋은 한 가지 타입의 관련된 속성과 파일들의 집합을 나타낸다
- 에셋 이름(Asset name)은 에셋에 접근하기 위해 개발자가 정의한 문자열
- 에셋 파일(Asset files)은 선택한 에셋의 데이터 파일 또는 리소스
- Attributes : 속성은 선택한 그룹, 에셋, 에셋파일의 속성(속성 인스펙터를 선택하면 볼 수 있다)

<center>
<figure>
<img src="/assets/post-img/swift/26.png" alt="" width="50%">
</figure>
</center>

- Asset variations : 위 그림에서 선택된 하나의 조각(에셋 파일들의 집합)을 나타낸다. 이 조각은 같은 속성 값(value)이 적용되는 단위



### 에셋 카탈로그의 세가지 타입

<center>
<figure>
<img src="/assets/post-img/swift/27.png" alt="" width="50%">
</figure>
</center>

- Folders : 에셋 카탈로그 폴더는 다른 그룹 폴더나 에셋 폴더를 포함할 수 있다.
  - 파일시스템의 폴더 이름은 대체적으로 확장자를 갖지 않지만 에셋 카탈로그 폴더는 위의 그림과 같이 해당 에셋 타입의 확장자가 자동으로 붙음
- JSON files : .json 확장자 파일로써 속성에 대한 정보를 포함
- Content files : 콘텐츠 파일은 리소스 파일을 나타냄


<center>
<figure>
<img src="/assets/post-img/swift/28.png" alt="" width="50%">
</figure>
</center>


이 그림은 에셋 카탈로그 폴더의 구조를 나타낸다.

- Asset catalog folder : 에셋 카탈로그 폴더는 모든 폴더와 파일들을 갖고 있다
- Group folder : 그룹 폴더는 다른 그룹 폴더나 에셋 폴더를 갖고 있다
- Asset folder : 에셋 폴더는 리소스 파일들을 갖고 있다


**프로젝트 내의 같은 타겟에는 에셋(폴더)의 이름은 반드시 고유해야 한다. 리소스 타입에 상관없이 이름은 고유해야 한다.**


## 앱 시닝과 앱 슬라이싱이란?

<center>
<figure>
<img src="/assets/post-img/swift/29.png" alt="" width="50%">
</figure>
</center>

- 앱 시닝(app thinning)이란?
앱 시닝이란 애플리케이션이 디바이스에 설치될 때 앱 스토어와 운영체제가 그 디바이스의 특성에 맞게 설치하도록 하는 설치 최적화 기술을 의미한다. 이를 통해 애플리케이션의 설치용량을 최소화하고 다운로드의 속도를 향상시킬 수 있다. 앱 시닝(app thinning)의 기술 구성요소는 **슬라이싱(slicing), 비트코드(bitcode), 주문형 리소스(on-demand resource)** 가 있습니다.

- 슬라이싱(slicing)이란?
슬라이싱(slicing)은 애플리케이션이 지원하는 다양한 디바이스에 대한 여러 조각의 애플리케이션 번들(app bundle)을 생성하고 디바이스에 알맞은 조각을 전달하는 기술이다. 개발자가 애플리케이션의 전체 버전을 iTunes Connect에 업로드하게 되면, 앱 스토어에는 각 디바이스 특성에 다양한 버전의 조각들이 생성된다. 사용자가 애플리케이션을 설치할 때 전체 버전이 아닌 슬라이싱(slicing)된 조각들 중 사용자의 디바이스의 가장 적합한 조각이 다운로드되어 설치되고, 에셋 카탈로그에서 관리하는 이미지들은 자동으로 적용이 된다.(슬라이싱(slicing)은 iOS 9.0 이상버전 이상만 지원한다)

> iTunes Connect란 개발자가 앱 스토어에 판매할 애플리케이션을 제출하고 관리할 수 있도록 도와주는 웹 기반 도구이다.

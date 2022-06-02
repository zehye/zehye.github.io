---
layout: post
title: iOS AppIcon 추가해보기 
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## AppIcon 추가해보기 

1. 1024px 이상의 
2. 사진은 정방형이여야 하고, 확장자는 png 이면서
3. 제일 중요하게 불투명한 배경을 가진 이미지인 앱 아이콘 이미지를 준비한다.


[App Icon Generator](https://appicon.co)

그리고 이 홈페이지에 접속을 하면 준비된 이미지를 드래그 해놓기만 하면 이미지들이 형성이 됩니다. 

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/38.png" alt="" width="80%">
</figure>
</center>

이렇게 다운로드 파일에 들어가 AppIcon 파일이 저장되어있는 것을 확인했다면 AppIcon.appiconset 안에 생성된 이미들이 종류별로 있음을 확인 가능할 것입니다.
이때 이 `AppIcon.appiconset` 파일을 통째로 프로젝트 내 assets에 임포트(import) 해줍니다. 

그리고 Xcode를 재실행(빌드) 해보고 나면 원했던 앱 아이콘이 제대로 적용 되어있음을 볼수있을 것 입니다. 


만약 반영이 안된다면, `Target > Build settings > Asset Catalog compiler - Options - Primary App Icon Set Name` 이 **AppIcon** 으로 되어있는지 확인을 해봅시다!
혹은 Assets에 있는 기본 AppIcon 파일을 지우고 직접 만들어 이미지를 임포트 해주면 추가가 되기도 합니다!


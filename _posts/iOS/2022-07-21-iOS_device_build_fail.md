---
layout: post
title: iOS Xcode 디바이스 빌드 실패(The operation couldn’t be completed) 해결방법
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Xcode 빌드 실패?

시뮬레이션에서도 문제 없었고 빌드도 잘되었는데, 아래와 같은 에러가 뜬다.

```
Could not launch "ProjectName"

The operation couldn’t be completed. Unable to launch opendoorLife.NavigationBarCheck because it has an invalid code signature, inadequate entitlements or its profile has not been explicitly trusted by the user.
```

실제 디바이스에서 설정 문제입니다 :)<br>
해결방법은 아래와 같습니다.

1. 아이폰 설정(Setting) > 일반(General) 클릭
2. VPN 및 기기 관리(Device Management) 클릭
3. 빌드 시키고자 하는 앱 선택
4. 개발자 신뢰를 위한~ 어쩌고 팝업 뜨면 **신뢰** 클릭

완료입니다
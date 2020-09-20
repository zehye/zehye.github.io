---
layout: post
title: app Transport Security has blocked a cleartext HTTP 에러 해결하기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## app Transport Security has blocked a cleartext HTTP

iOS 앱 개발 시 http 서버와 통신하려고 하면 구동중에 이런 에러가 발생한다.<br>
ATS 문제임을 알 수 있다.

```
App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

### Solution

Apple 측에서 앱 자체의 보안성을 위해 ATS(App Trasport Secuirty)라는 정책을 통해 기본적으로 https 통신을 하도록 유도하고 있는것이다.<br>
[ATS란 무엇인가?](https://www.zehye.kr/ios/2020/04/09/14iOS_ats/) > ATS가 무엇인지 모르겠다면 해당 포스팅을 읽어보도록 하자!

아무튼 http 서버로 테스트하기 위해선 **Info.plist** 에서 ATS 부분을 추가해주어야 한다.<br>
NSAppTransportSecurity의 NSAllowsArbitraryLoads 값을 true로 지정해준다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/20.png" alt="" width="80%">
</figure>
</center>

그러면 해당 에러가 해결될 것이다!

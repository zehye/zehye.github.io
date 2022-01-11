---
layout: post
title: iOS Firebase 8.8.0으로 업그레이드하면서 수정할 것들
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Firebase 8.8.0 업그레이드

Firebase를 7.10.0 에서 8.8.0으로 업그레이드 하였습니다.<br>
코코아팟을 사용했구요.


```vim
pod 'Firebase', '8.8.0'
pod 'Firebase/Core'
pod 'Firebase/Auth'
pod 'Firebase/Messaging'
pod 'Firebase/Analytics'
pod 'Firebase/Performance'
pod 'Firebase/Crashlytics'
```

했더니 아래와 같은 두개의 에러가 발생하였습니다.

- ld: framework not found FirebaseInstanceID
- ld: framework not found Protobuf

### 1. ld: framework not found FirebaseInstanceID

[참고한 Stackoverflow](https://stackoverflow.com/questions/62301690/framework-not-found-firebaseinstanceid-on-xcode)

#### Target > BuildSetting > 서치박스에서 FirebaseInstanceID 검색

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/30.png" alt="" width="80%">
</figure>
</center>

#### Linking에 FirebaseInstanceID가 있을 것

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/31.png" alt="" width="80%">
</figure>
</center>

#### Linking > Other Linker Flags > Debug/Release 두개 모두에서 -framework, FirebaseInstanceID delete(-) 클릭

<center>
<figure>
<img src="/assets/post-img/iOS/iOS3/32.png" alt="" width="80%">
</figure>
</center>

#### 반드시 Debug/Release 두개 모두에서 삭제해줘야하고
#### -framework, FirebaseInstanceID 이 두개도 반드시 지워줘야 함




### 2. ld: framework not found Protobuf

[참고한 Stackoverflow](https://stackoverflow.com/questions/59499381/framework-not-found-protobuf)

이 또한 위에 FirebaseInstanceID를 삭제해주는 방식과 동일하게 처리해주면 끝!

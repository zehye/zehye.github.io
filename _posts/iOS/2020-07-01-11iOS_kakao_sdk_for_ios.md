---
layout: post
title: Kakao API SDK v1 시작하기 for iOS
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

[kakao developer](https://developers.kakao.com/docs/latest/ko/getting-started/sdk-android-v1)


## Kakao API

삽질을 돌고돌아하긴 했지만, 제 글이 조금 불친절할 수도 있습니다.<br>
우선 환경이 iOS10기준으로 진행될 것이기 때문에 **iOS SDK v1** 으로 진행됩니다.

위 카카오 개발자 링크를 따라 들어가면 당연하게도 xcode와 kakao SDK 설치를 하여야 합니다.

개발 환경을 맞추기 위해(우리는 iOS개발을 하기에) xcode를 필수적으로 설치해야하고, 카카오 플랫폼 서비스에서 제공하는 기능을 사용하기 위해 카카오에서 제공해주는 kakao SDK를 설치해야겠지요~ kakao SDK를 설치해야할 때는 본인의 iOS 환경에 맞춰 설치하는 것이 중요합니다. 현재는 iOS13까지 나왔기에 최신 버전을 설치하면 되겠지만, 간혹 기업에서 제공하는 환경이 그 이하일 경우에는 그에 맞는 SDK 설치는 필수! 잘 확인합시다.


### 시작하기

#### 1. kakaoSDK 프레임워크 추가하기

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/37.png" alt="" width="50%">
</figure>
</center>

**Frameworks, Libraries and Embedded contents** 에 우리가 다운받은 SDK를 넣어줍니당<br>
여기서 중요한건 **KakaoOpenSDK.framework** 파일만 넣어주는 것입니다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/38.png" alt="" width="50%">
</figure>
</center>

이렇게 추가된 프레임워크가 잘 들어가져있는지 위 사진과 같이<br>
**[Target] > [build phase] > [Link Binary With Libraries]** 에서 확인해봅니다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/39.png" alt="" width="50%">
</figure>
</center>

그리고 Other Linker Flags에 "-all_load" 를 추가해야합니다. 이때 순서 및 주요한 부분은

1. [Target] > [Build settings]에 들어간다
2. [All] & [Combine]으로 체크되어있는지 확인하고(all로 체크되어있는지 확인하는 것이 중요)
3. 검색에 **other linker** 를 검색해
4. Other Linker Flags에 + 버튼을 눌러 **all_load** 추가해준다.


#### 2. 앱 등록하러 가기

[kakao developer 바로가기](https://developers.kakao.com/)

kakao developer로 들어가 로그인 후 내 어플리케이션 등록을 해줍니다. [내 어플리케이션] > [앱 만들기] 클릭!


<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/40.png" alt="" width="50%">
</figure>
</center>

사진은 굳이 등록하지 않아도 되며(지금은) 앱 이름과 회사 이름만 등록해주면 된다. 그러면 아래와 같이 앱 키가 만들어진다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/41.png" alt="" width="50%">
</figure>
</center>

그리고 이제 중요한 것! 플랫폼을 추가해주어야 한다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/42.png" alt="" width="50%">
</figure>
</center>

플랫폼 설정하기 클릭! 우리는 iOS 플랫폼을 설정할 것이기 때문에 **iOS 플랫폼 설정하기** 클릭합니다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/43.png" alt="" width="50%">
</figure>
</center>

필수적으로 적어줘야하는 것은 번들 ID인데 여기에는 우리가 만든 프로젝트의 번들 ID를 적어주면 된다. <br>
번들ID가 무엇인지 모르겠다면 **[Target] > [General] > [Bundle identifier]** 에서 확인하면 된다.

그리고 이제 또 중요한 부분!

다시 우리 프로젝트 xcode로 돌아와 **URL types** 를 추가해준다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/44.png" alt="" width="50%">
</figure>
</center>

[Target] > [Info] > [URL types]에 들어가 위 사진처럼 설정해주는데 이떄 중요한 것은

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/45.png" alt="" width="50%">
</figure>
</center>

URL Schemes에 **kakao** 문자열을 붙인 뒤 카카오 앱 키를 적어줘야 한다는 것이다.<br>
그리고 중요한 것은 이때 이 카카오 앱 키라는 것이 우리가 kakao developer에서 어플리케이션 등록을 하면서 받은 **네이티브 앱 키** 라는 것이다. 따라서 **kakao<내 어플리케이션의 네이티브앱 키>** 를 적어주면 된다.

그리고 이제 info.plist로 간다.


<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/46.png" alt="" width="50%">
</figure>
</center>

위 사진과 같이 **KAKAO_APP_KEY** 라는 키를 만들어 그 안에 밸류값으로 **네이티브 앱 키** 를 입력해준다.<br>
이때는 kakao 문자열을 붙이지 않아도 된다.

그리고... 아직 끝나지 않았다


#### 3. 화이트리스트 설정과 헤더파일 만들기

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/48.png" alt="" width="50%">
</figure>
</center>

info.plist를 [Open As] > [Source Code]로 열어준다. <br>
그리고 [kakao SDK 화이트리스트 설정](https://developers.kakao.com/docs/latest/ko/getting-started/sdk-ios-v1#white-list)으로 가서 해당 소스코드를 추가해준다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/49.png" alt="" width="50%">
</figure>
</center>

해당 스트링 값들을 추가해주면 되는데, 이때 처음 스트링값은 우리가 적어주었던 **kakao<네이티브앱 키>** 를 적어주면 된다.

그리고 이제 거의 마지막으로 다다르고 있는데! 이제 **bridge header** 를 만들어줍니다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/50.png" alt="" width="50%">
</figure>
</center>

말그대로 헤더 파일을 만들어주는 것이고 헤더파일을 만들었으면 <br>
[SDK Header 불러오기](https://developers.kakao.com/docs/latest/ko/getting-started/sdk-ios-v1#import) 해당 페이지에서 제공하는 소스코드를 복사붙여주기 해줍니다!


<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/51.png" alt="" width="50%">
</figure>
</center>


그리고 최종적으로 [Target] > [Build Settings] > [Swfit Compiler] & [Objective-C Bridging Header]에서 브릿지헤더파일의 경로를 잡아줍니다! 이떄 에러가 날 수 있는데, 해당 에러의 이유는 우리가 헤더파일에 SDK의 경로를 설정해주었는데 해당 경로에 SDK 파일이 존재하지 않기 때문이다.

즉, 우리 프로젝트 내에 SDK 파일이 존재해야한다는 것이다.

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/52.png" alt="" width="50%">
</figure>
</center>

위 사진과 같이 헤더파일이 가리키고 있는 곳에 SDK 파일이 존재해야한다는 것이다. <br>
그러면 에러없이 빌드가 되는 것을 볼 수 있을 것이다!

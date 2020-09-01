---
layout: post
title: This app could not be installed at this time 시뮬레이터 에러 대처하기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## This app could not be installed at this time

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/53.png" alt="" width="50%">
</figure>
</center>

xcode로 코딩을 하다 시뮬레이터를 돌릴때 이런 에러를 띄워주는 경우가 있다.<br>
수시로 업데이트 되는 개발도구이다보니 버그가 생겼을 수도 있고 특정 리소스가 엉켜서 발생하는 에러일 수도 있다.

그런데 위와 같은 에러는 빌드는 잘 되는데, **This app could not be installed at this time** 이 에러 문구만 띄울 뿐 시뮬레이터에서 디버그모드 실행이 되지 않고 있음을 보여주고 있다.

비슷한 문제를 겪은 사람들이 있어보여 첫번째 아래와 같은 방법을 실행해보았다.

### 1. 시뮬레이터 리셋

시뮬레이터 메뉴바상단에서 [Editor] > [Erase All Content and Settings] 실행

이렇게 하면 시뮬레이터가 잘 실행된다. 그런데 문제는 코드에서 또 무언가를 고친 뒤 다시 시뮬레이터를 실행하려하면 위와 같은 에러가 또 다시 발생한다는 것이다. 즉, 본질적인 문제가 해결되지 않음을 알 수 있었다.

[Stackoverflow 참고](https://stackoverflow.com/questions/47760643/xcode-this-app-could-not-be-installed-at-this-time)

### 2. Display Name을 영어로 변경

혹시 나처럼 앱의 Display Name이 한글로 되어있다면.... 자 영어로 한번 바꿔보도록 합시다.

그렇게 했더니 나는 놀랍게도 위와같은 에러가 더이상 발생하지 않았다....

[iPhone Dev 참고](https://iphonedev.co.kr/iOSDevQnA/126342)

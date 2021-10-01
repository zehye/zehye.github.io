---
layout: post
title: iOS/Xcode platform initialization failed 에러 해결하기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## platform initialization failed

프로젝트 빌드를 하려는데 갑자기 이런 에러가 발생했다.

[stackoverflow 같은 상황](https://stackoverflow.com/questions/47224594/error-at-launch-app)확인해보기



### 해결방법

```vim
cmd + shift + K > 클린
cmd + shift + option + K > 빌드 폴더 클린
```

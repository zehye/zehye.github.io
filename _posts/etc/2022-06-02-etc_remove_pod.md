---
layout: post
title: Xcode, 프로젝트에서 pod 삭제 후 재 적용해보기 
category: etc
tags: [Xcode]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## Xcode, 프로젝트에서 pod 삭제 후 재 적용해보기 

제 문제상황은 다음과 같았습니다.

1. 프로젝트의 이름을 바꿈
2. 이름을 변경하고 나니 pod install 이 제대로 실행이 되지 않았다.
3. 실제 모든 프로젝트의 이름을 변경했는데도 계속해서 실행이 되지않았다.
4. xcode 내부에서는 프로젝트명이 제대로 변경되어있었는데, 파인더를 통해 확인해보니 xcworkspace에 해당하는 파일명이 변경되지 않았다.
5. 강제로 변경하려고 하니 적용이 되지 않는다. 
6. pod 을 프로젝트에서 삭제하고 다시 실행해야겠다고 생각이 들었다. 


이에 대한 방법은 아래와 같았습니다.

1. 터미널에 `pod deintegrate` 입력
2. Podfile, Podfile.lock 파일 삭제
3. xcworkspace 파일 삭제
4. xcodeproj 파일을 열어 pod 관련 모든것 삭제
5. 다시 pod init, pod install 

이렇게 하고 다시 `open ~.xcoworkspace`를 했더니 `no scheme` 이 뜨더라.<br>
xcode를 cmd + q 를 통해 강제 종료하고 다시 열어보니 정상 작동!
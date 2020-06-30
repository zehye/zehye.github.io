---
layout: post
title: Build input file cannot be found 에러 해결하기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## error: Build input file cannot be found 에러

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/34.png" alt="" width="50%">
</figure>
</center>

info.plist 파일을 옮기게 되면 해당 에러가 발생한다.

에러내용은 위와 같이 빌드 입력 파일을 찾을 수 없다는 의미로, 프로젝트를 생성할때 자동적으로 Xcode 최상위의 info.plist파일을 생성하는데 내가 그 파일을 다른곳으로 옮겼기 때문에 찾을 수 없다고 에러를 띄워주는 것이다. 따라서 info.plist가 어디에 있는지 정확히 알려주면 된다.

### Solution 

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/35.png" alt="" width="50%">
</figure>
</center>

1. Xcode내 프로젝트 tarters의 buildsettions에서 infoplist라고 키워드를 검색
2. infoplist에 관한 항목들이 나오면 Info.plist 파일의 위치를 수정

---
layout: post
title: Build Success 했음에도 시뮬레이터가 돌아가지 않는 경우?
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Build Success 했음에도 시뮬레이터가 돌아가지 않는 경우?

빌드가 성공적으로 되었음에도 불구하고(**build success**) 시뮬레이터가 작동하지 않는 경우가 있다.


### 해결 방법

- [Product] > [Scheme] > [Edit Scheme] 혹은

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/58.png" alt="" width="80%">
</figure>
</center>


- [프로젝트] > [Edit Scheme]

<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/59.png" alt="" width="80%">
</figure>
</center>

위 방식으로 들어가서 [Run] > [Info] 에서 [Executable]에 **none** 이 되었는 지 확인<br>
만약 none이라면 본인의 프로젝트를 선택해준다! 이후 close!


<center>
<figure>
<img src="/assets/post-img/iOS/iOS2/60.png" alt="" width="80%">
</figure>
</center>

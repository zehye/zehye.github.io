---
layout: post
title: Xcode에서 remove reference한 파일 직접 삭제하는 방법
category: etc
tags: [etc]
comments: true
---

> 개인 공부 내용을 정리한 글입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!  

<hr>

## 문제 상황

실수로 xcode내 파일 중 하나를 삭제할 때 **move to trash** 를 하지 못하고 **remove reference** 를 클릭했다.<br>
동일한 이름의 파일을 올리려고 하니 이미 해당 파일이 폴더 내에 있어서 삭제가 안된다고 한다.

```
“GoogleService-Info (1).plist” couldn’t be moved to “SupportingFiles” because an item with the same name already exists.
```
Xcode내에는 이미 내가 파일을 삭제했으니 파일은 보이지않고, 근데 이미 파일은 존재한다고 하는데 어떻게 해야할까?


### 해결 방법

나 같은 경우는 해당 파일이 Supporting files라는 폴더안에 있었다. 따라서..

1. Supporting files 마우스커서 오른쪽 클릭
2. Show in finder 클릭
3. Supporting files 폴더를 들어가면 삭제되지 않은 파일들이 있음을 발견할 수 있다.

<center>
<figure>
<img src="/assets/post-img/etc/2.png" alt="" width="50%">
</figure>
</center>

원하는 파일을 직접 삭제하면 더이상 해당 에러는 발생하지 않는다!

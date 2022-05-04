---
layout: post
title: Xcode Error, Unable to boot device because it cannot be located on disk
category: etc
tags: [Xcode]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## Unable to boot device because it cannot be located on disk


이것 저것 삽질을 하던 중, 진짜 삽질을 했다.<br>
파인더에서 뭐가 엄청 지저분해보이길래.. 그냥 삭제를 했다. 

파일을 찾는데, 이거 삭제하고 다시 빌드를 하면 생기겠지.. 하는 안일한 생각과 함께 ..

그랬더니 `Unable to boot device because it cannot be located on disk` 이런 에러가 두두등장

검색해보니, 프로젝트를 실행해서 시뮬레이터를 재생하려는데 해당 시뮬레이터가 지정된 위치에 없을때 발생하는것이란다.<br>
고로 내가 시뮬레이터들을 모조리 삭제했다는 것을 의미하는 것 같은데.. 맞는 것 같았다 .ㅎ...


해결방법은 간단하다.

1. 현재 열려있는 Xcode와 시뮬레이터를 모두 종료시킨다.
2. 터미널을 키고 아래 명령어를 적어준다.


```vim
xcrun simctl erase all
```

그러면 에러 해결 완료!
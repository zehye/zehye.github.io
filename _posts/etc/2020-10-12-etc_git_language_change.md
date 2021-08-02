---
layout: post
title: Terminal에서 git 언어 설정 변경하기
category: etc
tags: [etc]
comments: true
---

> 개인 공부 내용을 정리한 글입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!  

<hr>

## 문제 상황

- 터미널에서 git이 계속 한글로 출력되어 나왔다.
- git을 사용할때 한글로 나오면 물론 편한점도 있겠지만 에러가 나는 경우
- 구글링을 해야하는 경우가 많은데, 한글로 나오는 에러는 구글에서 찾기가 쉽지 않다.
- 따라서 무엇을 사용하든 영어로 사용하는 것이 훨씬 좋다.

### 해결 방법

나는 zsh을 사용하기 때문에 zsh에서의 해결방식을 보여줄 것이지만<br>
bash에서도 설정 명령어는 같기에 그대로 진행해도 괜찮을 것이다.

우선 터미널에서 zshrc로 진입한다.

```
vi ~/.zshrc
```

그리고 아래 명령어를 적어준다

```
# Set Git language to English
alias git='LANG=en_GB git'
```

위 명령어 저장 후 나와서 터미널에 아래를 작성하면 끝!

```
source ~/.zshrc
```

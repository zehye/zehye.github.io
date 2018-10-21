---
layout: post
title: pull request 사용하는 방법
category: branch
tags: [git, branch]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

오랜만에 pull request를 사용하려고 하니, 헷갈리는 부분이 많아 정리하게 되었다.

우리가 보통 git을 통해 협업을 하다보면 각기 다른 branch를 나누어 이슈를 진행하고 이슈에 대한 테스트가 끝나고 나면 master에 올리기 전 다른 팀원들에게 최종 확인을 받듯이 `pull request`를 사용할 때가 존재한다. (사실 이전까지의 협업에서 pull request를 사용하지 않다보니 그새 사용하는 방법을 까먹었다..)

해당 미션을 수행해보자.

가정, 협업을 하는 과정에서 하나의 이슈가 발생해 `git branch dev1`이라는 branch에서 이슈를 진행한다고 가정한다.
```
git branch dev1

# 해당하는 이슈를 진행한 뒤
git add -A
git commit -m 'issue complete'

git push origin dev1
```
이렇게 함으로써 master에 push를 올리는 것이 아닌 dev1에 push를 올린다.

이후 해당 github의 저장소를 들어가게 되면


<center>
<figure>
<img src="/assets/git/git_pull_request.png" alt="" width="100%">
<figcaption>git pull request를 하는 방법</figcaption>
</figure>
</center>

이렇게 등장하는 것을 볼 수 있고 해당 버튼을 클릭하면

<center>
<figure>
<img src="/assets/git/git_pull_request2.png" alt="" width="100%">
<figcaption>git pull request를 하는 방법</figcaption>
</figure>
</center>

해당 이슈를 올리고 상대의 요청 응답을 기다리면 된다.


<center>
<figure>
<img src="/assets/git/git_pull_request3.png" alt="" width="100%">
<figcaption>git pull request를 하는 방법</figcaption>
</figure>
</center>

이럴때 상대가 직접 merge pull request를 해줄수 있다(내가 바꾼 코드에 이상이 없다면) 그러나 반대로 문제가 있을 경우 직접 고쳐줄 수도 있고 이에 대한 코멘트를 달아줄수도, 거절을 할 수도 있다.

반대로 내가 이 github에서 바로 conflict를 해결할 수도 있고 내가 직접 merge pull request를 할 수도 있다.

이렇게 하면 pull request를 하는 방법은 간단하다!

---
layout: post
title: git merge conflict - fast-forward와 3-way-merge
category: git
tags: [git, merge, fast-foaward, 3-way-merge]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## 문제상황

지금까지 git을 통한 협업 시 merge conflict(병합충돌)은 같은 파일의 같은 라인을 수정하면 무조건 일어나는 것이라고 생각했다. 그런데 병합 충돌이 일어나지 않는 경우를 발견했다.

merge를 하는 방식에는 `fast-forward`와 `3-way-merge` 두 가지가 있다.


### fast-forward

우선 `fast-forward`방식은 우선 아래의 그림에서부터 출발해본다.

<center>
<figure>
<img src="/assets/post-img/git/fw1.png" alt="" width="100%">
</figure>
</center>

여기서 C는 커밋(Commit)을 의미한다. 작업을 진행하며 위와같은 커밋이 진행되었고 현재 C2에 master가 존재하고 있다.

<center>
<figure>
<img src="/assets/post-img/git/fw3.png" alt="" width="100%">
</figure>
</center>

이때 이슈가 생겨 iss53이라는 브랜치를 생성하면 위와 같은 그림처럼 형성이 된다. 당연하게 iss53 브랜치에서 하는 행위 그 어떠한 것도 master에는 영향을 주지 않는다.

<center>
<figure>
<img src="/assets/post-img/git/fw3.png" alt="" width="100%">
</figure>
</center>

iss53 브랜치에서 이슈를 다 해결하고 커밋을 날리면 위와 같이 커밋 히스토리가 있을 것이다. 즉, master는 C2에 있지만 iss53은 C3를 가리키고 있다.

중요한 포인트가 뭐냐하면, C3의 커밋에는 C2까지의 내용이 모두 담겨있다는 사실이다. 이 상태에서 master로 merge를 하는 것은 단순히 master의 포인터를 최신 커밋을 가리키고 있는 C3로 옮기는 것을 의미한다.

현 브랜치가 iss53 브랜치 일 것이기에,

```vim
git ckeckout master
Switched to branch "iss53"

git merge iss53

Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
```

이렇게 merge 메시지에서 `fast-forward`가 보일 것이다.

C3 커밋이 C2커밋에 기반한 브랜치이기 때문에 브랜치 포인터는 merge 과정없이 그저 최신 커밋으로 이동을 한다. 이런 방식이 fast-forward 방식이다.

- A와 B 브랜치가 있다.
- A->B를 머지하려고 한다.
- B가 A 이후의 커밋을 가리키고 있다. (B의 조상커밋이 A이다.)
- A는 단순히 B와 동일한 커밋을 가리키도록 이동한다.

**즉, 단순히 포인터를 최신 커밋으로 옮기는 것을 fast-forward 방식이라고 한다.**


### 3-way-merge

이제 git에서 협업시 가장 많이 이루어지는 `3-way-merge`를 살펴보겠다.

<center>
<figure>
<img src="/assets/post-img/git/fw3.png" alt="" width="100%">
</figure>
</center>

다시 master 브랜치에서 이슈가 발생해 iss53 브랜치를 생성해 이슈를 해결하고 커밋을 날린 상황으로 돌아가 보겠다.

그런데 이때 master 브랜치에서 급하게 해결해야 될 핫 이슈가 발생했다.

<center>
<figure>
<img src="/assets/post-img/git/fw4.png" alt="" width="100%">
</figure>
</center>

그래서 master에서 hotfix 브랜치를 생성해서 급히 생긴 이슈를 해결하고 commit을 날린 후 master에서 merge를 했다.

즉, 이 상황은 아래와 같다.(iss53 시점에서부터 시작하겠다.)


```vim
git ckeckout master # iss53브랜치에서 master로 이동

git branch hotfix # hotfix브랜치 생성
git checkout hotfix # hotfix 브랜치로 이동

# 이슈 해결 뒤

git add -A
git commit -m 'hotfix 해결'

git ckeckout master # master 브랜치로 이동

git merge hotfix
```


<center>
<figure>
<img src="/assets/post-img/git/fw5.png" alt="" width="100%">
</figure>
</center>

그러면 이제 위 그림과 같이 master는 C4에 있게 되고 현재 포인터가 C4를 가리키게 된다.

이제 hotfix를 해결했으니 해당 브랜치는 삭제한다.


```vim
git branch -d hotfix
```

<center>
<figure>
<img src="/assets/post-img/git/fw6.png" alt="" width="100%">
</figure>
</center>

이제 다시 우리가 개발하던 iss53으로 돌아가 작업을 완료하고 커밋을 날려 C5라는 커밋 히스토리를 남겨보자.

이때 fast-forward와 다른 점이 보일 것이다.

현재 가리키는 커밋(C4-master)가 merge할 브랜치(C5-iss53)의 조상(C3)가 아니기 때문에 fast-forward 로 머지를 하지 않는다.


```vim
git checkout master

git merge iss53

Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
```

**즉, 3-way-merge는 각 브랜치가 가리키는 커밋 2개(C4, C5)와 공통의 조상 1개(C2)를 사용하는 것을 의미한다.**

<center>
<figure>
<img src="/assets/post-img/git/fw7.png" alt="" width="100%">
</figure>
</center>

3-way-merge의 최종결과는 위와 같다.

3-way-merge의 결과를 별도의 커밋으로 만들고 해당 브랜치가 그 커밋을 가리키도록 이동시킨다. 그 커밋은 C6이고 해당 커밋의 부모는 여러개가 되어있는 것을 볼 수 있다.


이때, 3-way-merge에서 merge-conflict가 발생할 수 있는 것이다. merge하는 두 브랜치에서 같은 파일의 같은 부분을 동시에 수정하게 되면 git은 해당 부분을 merge를 하지 못한다.

일반적으로 git은 자동적으로 merge를 하지 못하기 때문에 새로운 커밋또한 생성되지 않는다. 변경사항의 충돌은 개발자가 해결해주지 않는 한 merge 과정을 진행할 수도 없다.

그동안 병합충돌은 어느 방식에서든 같은 파일의 같은 곳을 수정하면 무조건적으로 일어나는 것이라고 생각했었다. 근데 해당 충돌이 일어나는 방식은 3-way-merge에서만 발생하는 것이란걸 알게되었다..

사실 지금도 매우 놀랍고 이해가 안가는 부분도 있지만.. 일단 그렇다고 한다. 만약 제가 이해한 부분이 틀렸다면 언제든지 댓글을 남겨주세요. 

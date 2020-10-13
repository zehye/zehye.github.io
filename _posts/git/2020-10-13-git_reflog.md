---
layout: post
title: git reset --hard origin/master 한거 취소하기(git reflog)
category: git
tags: [git, merge, fast-foaward, 3-way-merge]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## 문제상황

개발을 하던 중 대박 실수를 하나 했었다.

- local에 있는 파일을 깃헙에 push하려고 하는데 reject가 되었다.
- 이전에도 비슷한 충돌은 많아가지고 [git conflict해결](https://www.zehye.kr/project/2020/07/20/Project_git_conflict_solution/) 요거 참고하며 진행하는순간
- 초기 파일만 남은채 그냥 다 사라져버렸다. 아니지 초기파일도 아니고 그냥 텅빈 파일 이름들만 남아있었다....
- 구글링하며 설명을 찾아보니
- “만약, 로컬에 있는 모든 변경 내용과 확정본을 포기하려면 아래 명령으로 원격 저장소의 최신 이력을 가져오고, 로컬 master 브랜치가 저 이력을 가리키도록 할 수 있어요”
- 즉, 내 로컬은 텅 비어있던 깃헙을 역 push 해버려 텅빈 파일들만 남아있게 되었다...


### git reflog

ref는 reference를 의미한다 > **reference log**

<center>
<figure>
<img src="/assets/post-img/git/1.png" alt="" width="100%">
</figure>
</center>

- 사진을 보면 우선 위에서부터 아래로 커밋이 진행된 것을 볼 수 있다.
- 내가 뭐가 뭔지를 몰라서 reset이 좀 남발이 되어있는데
- 여튼 53d8에 있어야 할 HEAD가 잘못사용한 reset으로 인해 1b31까지 갔던 것을 볼 수 있다

이때 `git reset -hard HEAD@{몇번전의 행동}` 명령어를 통해 각각의 커밋 id를 대체할 수 있다.

즉 나는 git reflog를 통해 본 HEAD{3}행동으로 돌아가야 하니까(현재 HEAD{1}에 있고)<br>
`git reset -hard HEAD@{3}`이라는 명령어를 통해 커밋을 돌려놓았다.

즉 이 명령어를 통해 우리가 할 수 있는 일은 아래와 같다.

1. 원하는 몇번째 전의 행동으로 해당 과정을 되돌려 놓거나
2. reset으로 잘못 삭제한 커밋을 되살리기 위해 사용한다. 

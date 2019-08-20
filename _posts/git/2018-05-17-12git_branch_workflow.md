---
layout: post
title: git-branch의 workflow 이해하기 - push, fetch, remote repository
category: git
tags: [git, branch]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 `branch` `Workflow`에 대해 설명합니다.

주요 용어 : `push` `clone` `fetch` `pull`
<hr>

## Branch Workflow?

브랜치를 만들고 `merge`하는 것을 어디에 써먹어야 하는지 알아보자


**롱러닝 브런치**
`git`은 꼼꼼하게 `3-way-merge`를 사용하기 때문에 장기간에 걸쳐 한 브랜치를 다른 브랜치와 여러 번 `merge`하는것이 쉬운편이다. 그래서 개발 과정에서 필요한 용도에 따라 브랜치를 만들어 두고 계속 사용할 수 있다. 그리고 정기적으로 브랜치를 다른 브랜치로 `merge`한다.

이러한 접근법에 따라서 `git`개발자가 많이 선호하는 워크플로가 있는데, 그게 바로 `long-running workflow`이다.

이는, 배포했거나 배포할 코드, 즉 실제 운영환경에 나가는코드는 `master` 브랜치에 두고, 개발을 진행하고 안정화하는 브랜치는 `develop`이나 `next`라는 이름을 추가로 만들어 사용된다. 이러한 브랜치들은 언젠가 안정 상태가 되겠지만, 항상 안정상태를 유지해야하는 것은 아니다.

테스트를 거쳐 안정적이라고 판단되면 `master`브랜치에 `merge`한다. 그리고 현재 우리가 이슈를 다르고 있는 브랜치는 `iss53`브랜치로 이 같은 브랜치 또한 해당 토픽을 처리하고 테스트해서 버그가 없고 안정적이게 된다면 `merge`된다.


이제 `remote branch`를 만들어보도록 하자. (`local`이 아님을 반드시 기억하자)

우선 본인의 `git hub`에 들어가 `remote repository`를 만들도록 한다.

그리고 `zsh`에서

```
mkdir git-remotebranch
take git-remotebranch

git init

# http:// 부터의 주소는 본인의 리모트 저장소 주소를 의미한다.
git remote add http://~
# 리모트 저장소를 추가하기 위해서는 add를 사용하는데
# 앞에 있는 것은 우리가 별칭으로 사용할 리모트 저장소의 이름이 되고
# 뒤에 있는 것은 리모트 저장소의 주소가 된다.

git remote -v
```

하고나면

```
origin	https://github.com/zehye/Git-Remote-breanch.git (fetch)
origin	https://github.com/zehye/Git-Remote-breanch.git (push)
```

이는 이 저장소에 연결되어 있는 리모트 저장소를 나타내는 것이다.


일반적으로 리모트브랜치 이름은 (remote)/(branch) 형식으로 나타난다. 예로들어 리모트 저장소의 `origin`의 `master`브랜치를 보고 싶다면 `origin/master`라는 이름으로 브랜치를 확인하면 된다.

다른 팀원과 함께 어떤 이슈를 구현할 때 그 팀원이 `iss53`이라는 브랜치를 서버로 `push`했고 나 또한 로컬에 `iss53`이라는 브랜치가 있다고 가정하자면, 이때 서버의 `iss53` 브랜치가 가리키는 컷은 로컬에서 `origin/iss53`이 가리키는 커밋이다.

예로 들어 `git.ourcompany.com`이라는 git 서버가 있고 이 서버의 저장소를 하나 clone하면 git은 자동으로 `origin`이라는 이름을 붙인다. `origin`으로부터 저장소 데이터를 모두 내려받고 `master`브랜치를 가리키는 포인터를 만든다. 이 포인터는 `origin/master`를 가리키게 하고 이제 이 `master`브랜치에서 작업을 시작할 수 있다.


```
git push origin master

# clone하는 방법은 -> git clone 디렉토리 주소
git clone ~
```

이렇게 `clone`하게 되면 cd, remote-v, branch-v, git log 모두 다 동일하게 나온다.
그리고 위에서 말했듯 클론 받은 저장소가 `origin` 으로 된다.

이때 클론 받은 사람이 파일을 수정해본다.

```
vi README.md
git add README.me
git commit -m '~'

```

이를 두개 만들어서 log를 통해 파악해보면 `HEAD/master`와 `origin/master`가 다르게 들어가 있음을 볼 수 있다.

이때 master에 origin/master의 내용을 보여주고 싶다면

```
git push origin master
```

`git log`에 들어가 확인해보면 한 곳에 브랜치가 다 모여있다.

그런데 이는 origin 저장소에 변경된 내용을 로컬 워킹 디렉토리에 변경사항만 가져온 것으로 바로 적용이 되는 것은 아니다.

이를 합치려면 (내가 만든 커밋과 상대가 만든 커밋을 합쳐야 하니까)

그때
```
git fetch origin
```

근데 이때
```
git merge origin/master
```

하려고 하면 충돌이 생기고 그 충돌은 앞에서 했던 것처럼 해결한 뒤
```
vi ~

git add ~
git commit -m '~'

git push origin master
```

해결!


**참고**
remote repository 이름 바꾸는 방법

```
git remote rename ~
```

remote repository 삭제하는 방법

```
git remote remove ~
```

근데 저장소를 삭제한다는 의미는 로컬에서의 연결만 지워지는 것이지 그 저장소는 그대로 남아있다.

---
layout: post
title:  git-branch 이해하기
categories: [branch]
---
이 포스팅에서는 `branch`에 대해 공부해본다
<hr>

## Branch?


일반적으로 `운영환경`은 프로그램에 의해 돌아가는 서비스라고 하는데, 즉 실제 유저가 사용하고 있는 프로그램 상태를 의미한다.

그 운영환경에 사용되는 소스코드들이 존재하는데, 우리는 해당 소스코드를 마음대로 고쳐쓸 수는 없다.
왜냐하면 아직 수정된 코드에 버그가 있는지 확인하지도 않은 상태에서 운영환겨에 그대로 적용이 된다면, 실제 사용자가 사용할 때 바로 에러가 발생하게 되니까!

따라서 우리는 운영코드는 그대로 두고, 소스코드를 통째로 따와 그 곳에서 개발을 하고 실험을 한 뒤, 더이상 안정화가 되었다고 생각이 들었을 때, 변경사항을 운영코드에 올리도록 한다.

그런데 사실, 어떤 프로젝트를 개발한다고 했을때, 여러명의 개발자들이 자신만의 독자적인 개발을 진행하는데, 단 하나의 저장소만을 사용한다고 하면 너무 번거롭기 때문에 개발자들이 나눠서 개발을 하다가 나중에 합치는 활동을 하는데

**그 과정이 마치 여러 갈래로 나뉘는 것처럼 보여 그것을 `branch`브랜치라고 한다.**

이러한 `branch`는 `git`의 `commit` 사이를 가볍게 이동할 수 있는 어떤 `포인터`같은 것으로 브랜치는 특정 커밋을 가리키는 역할만 한다.

이러한 `git`에서는 `master`라는 브랜치 하나를 기본적으로 만드는데, 이 같이 `master`브랜치를 가지고 있는 이유는 이 브랜치가 가리키는 어떤 스냅샷, 즉 시점에 맞게 `working directory`를 보여주기 때문이다.


#### 1. branch 만들기

```
git branch testing
# testing은 branch의 이름이다.
```

이때 `HEAD`는 master와 testing 을 가리키고 있고 그 의미는 즉슨, 같은 commit을 가리키고 있음을 뜻한다.

`HEAD`는 지금 작업중인 `branch`가 무엇인지를 알려준다. 이 헤드 포인터는 지금 작업하고 있는 `local branch`를 의미한다. 즉 `zsh`에서 `master`라고 표시되어 있는 이유는 지금 `HEAD`가 어디에 위치하고 있는지를 바로 보여주는 것이다.


#### 1-1. 브랜치 위치 확인하기

```
git branch -v
```

이떄 `git branch`명령은 새로운 브랜치를 만들기는 하지만, `HEAD`를 옮기지는 않는다.


#### 1-2. 브랜치 이동하기

```
git checkout testing
```

이는 `master`에 있던 브랜치를 `testing`로 옮긴다.


```
cd ~/
mkdir branch
cd branch
git init

git status
(no commit yet)

echo README > README
echo test.rb > test.rb
echo LICENSE > LICENSE

git add README test.rb LICENSE
git commit -m 'The initial commit of my project'

echo 'change!!!' > test.rb
Git add test.rb
git commit -m 'made a change!'

git log --oneline
```

`testing`와 `master`가 향하는 위치가 다름을 파악할 수 있다.



**여기서 잠깐**
`HEAD`, `tag`, `branch`는 모두 커밋을 가리키는 기능은 전부 있다.


우선, `tag`는 딱 한개의 `commit`만을 가지는 것이고, `HEAD`는 워킹디렉토리가 어떤 스냅샷을 보여줄거냐를 중점으로 둔다.
근데 `branch`는 작업을 나눠서 하고 싶을 때 브랜치는 가리키는 커밋으로 가서 작업을 하는 것이다. 즉 남기는 커밋마다 브랜치는 이동이 가능하다.

> 우리가 어떤 커밋을 만드냐에 따라 브랜치는 그 커밋을 계속 따라간다.

`master`는 운영되고 있는 코드라고 하고, 새로운 코드를 만들고 싶어서 `testing branch`를 만들어 거기서 작업을 하면
브랜치는 계속 갈려 나아가는데 이 작업에 만약 `branch`가 아닌 `tag`를 붙여서 작업을 한다는 것은 아예 의미가 없다.

> 태그는 태그에 헤드를 가져다대는것이 그 자체가 불가능하다고 생각하면 된다.
물론 가능은 한데, 다음 작업을 할때 같이 이동을 한다는 것 자체가 없다.

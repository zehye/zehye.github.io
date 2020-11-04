---
layout: post
title: git 사용하면서 발생한 에러 총정리 + 해결방안 및 저장소 복사하는 방법
category: git
tags: [git]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## Git 에러 그 시작....

> 그냥 개인적인 해결방법입니다. 참고를 하시되 전혀 스마트한 방법이 아님을 꼭 강조하겠습니다.

우선 나는 터미널에서 git만 사용했지 git-flow는 사용해보지 못했다.<br>
사실 git을 쓰든 git-flow를 쓰든 큰 차이가 있는것도 아니고, git-flow도 조금만 찾아보면 사용방법이 무척 잘 정리되어있고 간단해서 그리 큰 어려움이 있지는 않았다. 다만 문제는 들어온 회사의 전 개발자가 git branch 정리를 제대로 해놓고 가지 않았던 것... 그것부터가 재앙의 시작이었다.

사실 문제를 해결한 지금 시점에서 그리 큰 문제는 아니었지만, 첫 개발회사를 다니게 된 나의 입장에서는 git fatal error가 뜰때마다 심장이 쪼그라드는 기분이었달까..?

사수로부터 전 개발자가 release 브래치를 finish하지 않았기 때문에 master, develop 브랜치에 병합을 해야한다고 말을 해주었다.<br>
release 브랜치를 finish 하기만하면 자동으로 master, develop 브랜치에 병합된다는것을 알고 있었기에 자연스럽게 아래 코드를 작성하였다.

```
git flow release finish <해당 버전>
```

그러나 에러가 뚜둔

```
Branches 'release/0.4.0' and 'origin/release/0.4.0' have diverged.
And local branch 'release/0.4.0' is ahead of 'origin/release/0.4.0'.
Branches 'develop' and 'origin/develop' have diverged.
And local branch 'develop' is ahead of 'origin/develop'.
Fatal: Tag already exists and does not point to release branch 'release/0.4.0'
```

우선 첫 네줄은 간단하게 지금 release, develop 브랜치에 변동사항이 있었기에 git add와 commit 을 남겨주기만 하면 해결이 된다. <br>
단 문제는 마지막 `Fatal: Tag already exists and does not point to release branch 'release/0.4.0'`<br>
저 태그는 우선 내가 만들어준 태그는 아니었고, 이전에 만들어진 hotfix 0.4.1 을 의미하는데 이를 해결할 방법이 마땅치 않았다.

당시에 해결했던 방법으로는 태그를 업데이트 해보거나, 푸시해보거나 그런 방법이었는데 모든것은 다 무용지물!

```
git push --tags
git pull --tags
git push origin --tags
git push --tags -f
git fetch --tags
```

위 코드를 순서대로 진행하라는 의미는 아니고, 당시에 stackoverflow를 보며 찾아 실행했던 모든 명령어들을 그냥 적어놓은 것 뿐이당..<br>
그러니 저 순서대로 참고하지는 말것!

여튼 그럼에도 불구하고 해결되지 않았고 머지를 하려해도 계속 충돌충돌<br>
깃을 항상 혼자만 써와서 그런지(협업을 했다고 하더라도 히스토리 하나도 모른상태로 깃을 사용한적은 없기에..) 너무 어려웠다.<br>
더군다나 개인 플젝도 아니고 회사 플젝이다보니 내가 잘못건들여서 망하면 어떡해! 하는 마음에..(쫄쫄보)

그래서 결국 내가 선택한 방법은 해당 프로젝트의 저장소를 내 저장소로 복사해와서 내 저장소 안에서 실행해보는것!


### 저장소 복사해보기

방식은 해당 블로그를 참고해서 보았다.. 제드님 정말 최고 >> [참고블로그](https://zeddios.tistory.com/5)

1. 터미널 내에서 폴더를 하나 만들어준다.
2. 내가 만들어놓은 폴더에 해당 코드 작성 >> `git clone --mirror <복사할 저장소의 url>`
3. 복사된 저장소 파일로 들어가 내 저장소 주소 입력 >> `git remote set-url --push origin <내 저장소의 url>`
4. 그리고 push! >> `git push --mirror`
5. 제대로 내 저장소에 복사되었는 지 확인 >> `git remote -v`

그런데 나는 저장소에 제대로 복사가 되었음에도 불구하고 `git push --mirror`를 하면 계속해서 에러가 발생했다.

<center>
<figure>
<img src="/assets/post-img/git/2.png" alt="" width="100%">
</figure>
</center>

요렇게..ㅠ 그래서 또 stackoverflow를 열심히 찾아보았다. >> [참고한 stackoverflow](https://stackoverflow.com/questions/34265266/remote-rejected-errors-after-mirroring-a-git-repository)<br>
근데 사실 큰 방법을 얻어내지는 못했다. 그래서 일단..그래 저장소에 옮겨서 확인하는 것은 포기!<br>
내 깃헙 저장소로 돌아와 conflict를 최대한으로 해결해보았지만, 너무 많은곳에서 conflict가 발생해서 웹상에서 해결하는것이 불가능하다고 하기에 깃헙에서 추천해준 방법을 사용해서 해결하려고 하였으나 이 방법 또한 터미널에서 거절당했다. 부들부들..

어떻게 해야할지 고민하다가 내린 결론은 그냥 다 merge를 시켜버리자.

깃 에러를 해결하는 과정에서 각각의 브랜치의 진행 상태를 어느정도 파악했었고, release에서 develop과 master에 병합을 하라고 했으니 현재 상태는 release가 가장 최신의 상태임을 확인할 수 있었다. (실제 코드상으로도 소스트리를 통해 본 결과에서도 master와 develop은 한참 뒤에 멈춰있었고, 이전 개발자는 release로 계속해서 개발을 진행했던 것으로 확인했다.)

더 나아가, 현재 git flow release finish가 되지 않는 상황이니 수동으로 merge를 진행하는것이 가장 최선이라고 생각했다.<br>
더 좋은 방법이 있다면 혹은 해결방법을 알고 있는 분이 계시다면 꼭 ! 알려주세요.... 정석적인 방법을 알고 싶습니다..

그래서 그냥 정말 수동으로 다 머지를 시켰다.<br>
정말 다른 방법은 없고 release 브랜치로 들어가 `pod install`을 알맞게 맞춰서 한 다음 git add, commit을 진행하고<br>
git checkout develop으로 develop 브랜치로 들어가 `git merge release`를 해주었다.

이후 develop브랜치에서 제대로 release버전이 병합된 것을 확인헀고, 여기서부터 다시 git flow feature를 통해 새로운 브랜치를 생성해 앞으로 개발을 해나갈 예정이다. 정석적인 방법도 아니고 아직 제대로 된 해결방법도 모르겠지만... 지금으로써는 이게 최선이었다. 초기 셋팅 너무나 빡쎄네용...

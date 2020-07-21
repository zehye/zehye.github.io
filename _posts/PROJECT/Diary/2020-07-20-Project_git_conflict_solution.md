---
layout: post
title: 다이어리 앱 만들기 - 깃 에러 해결하기(set upstream, non fast-forward)
category: PROJECT
tags: [PROJECT]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

실수로 .git 파일을 삭제하게 되면서 해당 깃에 포함되어있던 브랜치를 다 날려버리는 바보같은 짓을 저질렀다.....멍청

다시 깃을 연결하여 진행하려다 보니 아래와 같은 에러가 발생헀다.

```
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use
    git push --set-upstream origin master
```

> 치명적 : 현재 분기 마스터에 업스트림 브랜치가 없습니다.        
현재 브랜치를 푸시하고 리모트를 업스트림으로 설정하려면 다음을 사용하십시오.

위와 같은 내용으로 우선 에러가 말해준 대로 아래를 진행해준다.

```
git push --set-upstream origin master
```

그러고 나니 아래와 같은 에러가 발생했다.

```
! [rejected]        master -> master (non-fast-forward)
error: 레퍼런스를 'git@github.com:zehye/diary.git'에 푸시하는데 실패했습니다
힌트: 현재 브랜치의 끝이 리모트 브랜치보다 뒤에 있으므로 업데이트가
힌트: 거부되었습니다. 푸시하기 전에 ('git pull ...' 등 명령으로) 리모트
힌트: 변경 사항을 포함하십시오.
힌트: 자세한 정보는 'git push --help'의 "Note about fast-forwards' 부분을
힌트: 참고하십시오.
```

엇 이 에러는 예전에 자주 발생하여 정리해놓은 글이 있다. 참고> [non fast forward 에러 해결하기](https://www.zehye.kr/git/2019/10/27/11git_push_error/)

그래서 아래와 같이 진행해보았다.

```
git pull origin master --allow-unrelated-histories
```

근데 또 에러가 발생했다.

```
* branch            master     -> FETCH_HEAD
error: 병합 때문에 추적하지 않는 다음 작업 폴더의 파일을 덮어씁니다:
```

그래서 아래와 같이 진행헀다.

```
// origin을 가져오고
git fetch --all

// HEAD의 현 위치를 바꿔준다
git reset --hard origin/master
```

그러고 다시

```
git push --set-upstream origin master
```

위 문장을 실행해주면 문제없이 push 되어 올라가는 것을 볼 수 있다!

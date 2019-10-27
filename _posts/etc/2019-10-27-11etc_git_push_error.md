---
layout: post
title: non-fast-forward 에러 해결하기
category: etc
tags: [ubuntu, pyenv, python]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!  


<hr>


## 문제 상황

- github에서 저장소 생성 후 저장소 주소를 remote에 입력(git remote add origin https://github…..)
- 로컬에서도 정상적으로 초기화(git init)
- git pull 또는 git merge 명령이 동작하지 않음
- git push origin master시 `[rejected] master -> master (non-fast-forward)`에러 발생


```zsh
git push -u origin master  

To github.com:
! [rejected]        master -> master (non-fast-forward)

error: 레퍼런스를 'git@github.com:'에 푸시하는데 실패했습니다
힌트: 현재 브랜치의 끝이 리모트 브랜치보다 뒤에 있으므로 업데이트가
힌트: 거부되었습니다. 푸시하기 전에 ('git pull ...' 등 명령으로) 리모트
힌트: 변경 사항을 포함하십시오.
힌트: 자세한 정보는 'git push --help'의 "Note about fast-forwards' 부분을
힌트: 참고하십시오.
```

## 원인

깃헙에 생성된 원격 저장소와 로컬에 생성된 저장소 간 공통분모가 없는 상태에서 병합하려는 시도로 인해 발생.<br>
기본적으로 관련 없는 두 저장소를 병합하는 것은 안되도록 설정되어 있음.

## 해결방법

아래와 같이 git pull 시에 –allow-unrelated-histories 옵션 추가하여 관련 없었던 두 저장소를 병합하도록 허용

`git pull origin master --allow-unrelated-histories`

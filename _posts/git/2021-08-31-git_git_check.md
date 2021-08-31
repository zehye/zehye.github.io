---
layout: post
title: 자주 사용하지만 헷갈리는 git 명령어 정리해보기
category: git
tags: [git]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## git branch 생성하기

```
git branch [만들 브랜치 이름]
```

## git branch 확인하기

```
git branch -r  // 원격 저장소 브랜치 리스트 확인
git branch -a  // 원격, 로컬 모든 저장소의 브랜치 리스트 확인
```

## git branch 가져오기

```
git checkout -t [가져올 브랜치 경로/이름]
```

## git branch 이름 변경하기

```
git branch -m [기존 브랜치 이름] [바꾸고 싶은 브랜치 이름]
```

---
layout: post
title: 맥 초기 설정사항
category: etc
tags: [python, computer science, django]
comments: true
---

> 이외의 참고할 만한 사이트 [leesoo's blog](https://leesoo7595.github.io/2018/10/06/macOS_development_environment/)

<hr>

## 맥 변경사항

### 단축키 변경

  - 한/영 전환: Shift + Space
  - Spotlight: Ctrl + `
  - 바탕화면보기: F1
  - 전체 창 목록 보기: F3
  - 현재 실행중인 프로그램의 창 목록 보기: F4

### Moom (단축키로 창 크기 바꾸기)

  - 우측 위 M아이콘
  - 왼쪽에 창 배치: alt + <-
  - 오른쪽에 창 배치: alt + ->
  - 1/4로 배열: alt + (1,2,3,4)
  - 그외 단축키는 직접 만들어보기 ><

### Command Line Tools설치

xcode-select --install

### brew설치

앞으로는 프로그래밍 라이브러리들은 dmg등 이미지 받아서 설치하지 말 것 (IDE같은 라이브러리 외 유틸리티는 무관)

[참조사이트](https://brew.sh/index_ko)

### brew로 라이브러리 설치

- brew install git git-lfs

### git 설정 (나중에 할 것)

git config --global user.name "Your Name"

git config --global user.email "you@your-domain.com"

git config --global core.precomposeunicode true

git config --global core.quotepath false

### zsh설치

1. brew install zsh
2. oh-my-zsh 설치
    1. [참조사이트](https://github.com/robbyrussell/oh-my-zsh)
    2. via curl부분 사용
3. 터미널 프로파일 변경 (Basic -> Pro)
4. vi ~/.zshrc
    1. 두 번째줄의 주석 해제
    2. ` # export PATH=$HOME/bin:/usr/local/bin:$PATH`

### Node.js설치

brew install node

(업데이트시에는 brew upgrade node또는 brew upgrade만 치면 끝)

### brew cask로 프로그램 설치

일반 애플리케이션도 brew cask로 제공되는게 있다면 그걸로 설치

- brew cask install google-chrome
- brew cask install iterm2
- brew cask install vlc
- brew cask install atom
- brew cask install slack
- brew cask install pycharm

### 터미널 개발환경 세팅

[참조사이트](https://subicura.com/2017/11/22/mac-os-development-environment-setup.html)

### shell 관련 라이브러리

- brew install zsh-completion
- brew install zsh-autosuggestions
- brew install zsh-syntax-highlighting
- brew install zsh-history-substring-search

### 추천 프로그램

- 메일
    - Spark: 기본 메일앱보다 훠어어어어어어얼씬 좋고 폰이랑 자동연동
- 마크다운 편집기
    - MacDown: 마크다운
- 클라우드
    - Dropbox: 실시간 연동

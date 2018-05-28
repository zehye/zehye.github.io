---
layout: post
title:  rbenv환경에서 Jekyll 블로그 생성하고 GitHub Pages에 배포하기
categories: [jekyll]
---

이 포스팅에서는 `macOS`환경에서 `rbnv`를 사용해 `Jekyll`블로그를 생성하고,<br>
이를 `GitHub Pages`에 배포하는 방법을 다룬다.
<hr>

## Jekyll?

`Jekyll`은 일반적으로 블로그를 만들기 위해 사용하는 정적 사이트 생성기이다.<br>
데이터베이스를 사용하지 않고 `git`과 같은 버전관리 시스템을 사용해 포스트들을 관리하며, 정적 웹사이트이기 때문에 단순 파일 서빙만으로 블로그를 만들 수 있다.

정적 웹 사이트라는 특징덕에 `GitHub`에서는 `Jekyll`블로그를 서빙하기 위한 시스템을 제공하며 무료로 사용가능하다.

또한 포스트(소스)의 형태를  `HTML`이 아닌 개발자 친화적인 다른 마크업(대부분의 경우 `Markdown`) 사용할 수 있으며, 데이터베이스를 쓰지 않는다는 점은 역으로 버전관리 시스템을 사용해서 작성한 글 들을 편리하게 관리할 수 있다는 장점을 준다.

데이터베이스를 쓰지 않기 때문에 댓글기능은 타사 서비스를 사용하서나 (`disqus`) 직접 만들어야 한다.

### 설치

`Jekyll`을 로컬환경에서 실행하기 위해서는 특정 버전 이상의 `Ruby`가 필요하다. `macOS`에 기본설치된 `Ruby`는 버전이 낮기때문에, 시스템의 `Ruby`를 업그레이드하거나 `Ruby`의 버전관리도구를 사용하는 방법이 있다.<br>
개인적으로는 시스템에 이미 설치된 파이썬이나 루비를 변경하는 것을 선호하지 않기 때문에, 여기서는 `Ruby`의 버전관리도구  하나인 `rbenv`를 사용한다.

#### Homebrew 설치

`rbenv`를 설치하기 위해 macOS용 패키지 관리자인 `Homebrew`를 이용한다.
```
/usr/bin/reby -e "$(surl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install))"
```

* `Homebrew`에 대한 자세한 내용은 공식 페이지에서 확인할 수 있다.
* 이미 `Homebrew`가 설치되어 있는 경우 `brew update`를 이용해 `Homebrew`를 최신버전으로 업데이트해준다.

#### 이미 설치되어 있던 ruby 삭제
이미 `Homebrew`를 사용하던 경우, 앞으로 `rbenv`에서 `Ruby`의 실행을 관리할 것이므로 기존에 설치된 `Ruby`를 지워준다.
```
brew uninstall Ruby
```
#### rbenv 설치
```
brew install rbenv ryby-build
```
#### rbenv를 위한 설정을 셸 설정파일에 추가
`zsh`을 쓰는 경우 `~/.zshrc`, 기본 `bash`를 쓰는 경우 `~/.bash_profile`에 작성한다.
```
# rbenv
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
```
**설정파일을 작성한 후 터미널을 종료하고 새로 열어준다**



#### rbenv를 이용해 ruby설치, 전역에서 사용할 ruby 버전 지정
```
➜ rbenv install 2.4.1
➜ rbenv global 2.4.1
➜ rbenv versions
  system
* 2.4.1 (set by /Users/lhy/.rbenv/version)
```
`*`표가 붙은 부분이 현재 사용하고 있는 `Ruby`버전을 나타낸다.
나는 초기에 2.4.1버전을 설치했는데 현재는 2.5.1까지도 나와있으니 해당버전으로 깔아도 될듯하다.

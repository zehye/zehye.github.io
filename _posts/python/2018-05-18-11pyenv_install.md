---
layout: post
title: pyenv, virtualenv, iPython 설치 및 설정하기
category: python
tags: [python, pyenv, virtualenv, iPython]
comments: true

---

> 패스트캠퍼스 컴퓨터공학 입문 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.   


## Python?


`python`은 흔히 고급언어라 불리고 고급언어란 사람이 알아듣기 매우 쉬운 언어를 의미한다.


`python`을 하기 위해서는 `pyenv`를 설치해야 하는데 `pyenv`는 프로젝트별로 파이썬 버전을 따로 관리할 수 있도록 도와주는 라이브러리를 뜻한다.

여러 프로젝트를 동시에 진행하다보면, 어떤 프로젝트에서는 `python 2.7`을, 어떤 프로젝트에서는 `python 3.4`를 사용하는 식으로 다양한 버전을 사용할 수 있는데, 이런 때 각각에 설치된 라이브러리 간 충돌을 방지하기 위해 사용한다.


### pyenv 설치

```
* 맥

brew install pyenv
brew install pyenv-virtualenv
```

이때 `virtualenv`는 파이썬 개발환경을 프로젝트별로 분리해서 관리할 수 있게 해주는 라이브러리다

위의 `pyenv`와 다른점은 `pyenv`는 파이썬의 버전을 관리해주는 거라면, `virtualenv`는 **파이썬 패키지 설치 환경** 을 따로 관리한다


### pyenv 설정

```
* 설치 후 pyenv 관련 설정을 shell에 추가한다
vi ~/.zshrc

export PYENV_ROOT=/usr/local/var/pyenv
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi
if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi

```

그리고 pyenv를 통해서 `python 3.6.5`버전을 설치한다.

`pyenv install 3.6.5`



### pyenv 사용
<hr>

1.가상환경 생성

`pyenv wirtualenv <version> <env_name>`

> pyenv virtualenv 3.6.5 new_python

2.사용할 폴더로 이동

```
cd projects
mkdir python
cd python
```

3.local 에 가상환경 지정

`pyenv local now_python`

4.가상환경 지우기

`pyenv uninstall new_python`



###iPython

기본 파이썬 셸보다 다양한 기능을 할 수 있도록 해주는 셸을 제공한다.



### iPython 설치

`pip install iPython`

커맨드 라인에서 `ipython`실행


**vi 단축기**

`shift + g` : 가장 아래로

`shift + a` : 현재 줄에서 가장 마지막으로

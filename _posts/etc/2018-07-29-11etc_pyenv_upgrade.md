---
layout: post
title: pyenv를 통해 파이썬 최신 버전 설치하기
category: Docker
tags: [docker]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

**pyenv는 시스템 단위에서 이루어지기 때문에 가상환경 안에서하든, 밖에서 하든 상관없다.**

### Pipfile

[requires]
python_version = "3.7"

### shell

- `pyenv versions`를 통해 현재 깔려있는 python 버전을 확인해본다.
- 3.6.5만 깔려있는 경우, `pyenv install --list`를 통해 몇 버전까지 깔 수 있는 지 확인해본다.
- 3.7.0이 없는경우 `pyenv update`를 해줘야한다.
- pyenv를 통해 보면 `update`라고 써있을 텐데, 만약 `brew`를 통해 깐 경우 update는 없다.
- `brew upgrade pyenv`를 해준다.
- update가 있는 경우, `pyenv update`
- 그러고 pyenv install --list를 통해 3.7.0을 확인하고 `pyenv install 3.7.0`

---
layout: post
title: Ubuntu에서 pyenv 통해 python 설치시 발생하는 에러 (zipimport.ZipImportError)
category: etc
tags: [ubuntu, pyenv, python]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!  


<hr>


ec2 연결과정에서 ubuntu에 접속 후 pyenv 를 통해 python을 설치하려고 하자 아래와 같은 에러가 발생했다.

```shell
~ pyenv install 3.7.1
zipimport.ZipImportError: cant decompress data; zlib not available
Makefile:1099: recipe for target 'install' failed
make: *** [install] Error 1

```

일반적인 경우에 (ubuntu환경이 아닌경우) 이러한 에러가 발생하는 이유는

Xcode Command Line Tools가 기본적으로 설치해주던 라이브러리 패키지를 설치해주지 않게 바뀌어 발생하는 문제라고 한다.

따라서 이 라이브러리 패키지를 깔아주면 되고 해당 환경에 맞춰서 install 해주면 된다.

[pyenv Common build problem](https://github.com/pyenv/pyenv/wiki/common-build-problems)

해당 링크를 통해 들어가 ubuntu 환경에 맞는


- Ubuntu
```
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
```

설치 완료 후 `pyenv install 3.7.1`을 해주면 안정적으로 파이썬 설치가 완료되었다.

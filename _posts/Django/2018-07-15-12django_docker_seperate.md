---
layout: post
title: Django - Dockerfile 분리해서 실행하기
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## Dockerfile.base

```Dockerfile
FROM                ubuntu:16.04
MAINTAINER          <본인 이메일>

# update, upgrade
RUN                 apt -y update && apt -y dist-upgrade

# zsh
RUN                 apt -y install zsh curl git
RUN                 curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | bash
RUN                 chsh -s /usr/bin/zsh

# pyenv
RUN                 curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
RUN                 apt -y install make build-essential libssl-dev zlib1g-dev libbz2-dev \
                           libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
                           xz-utils tk-dev libffi-dev

# pyenv settings
RUN                 echo 'export PATH="~/.pyenv/bin:$PATH"' >> ~/.zshrc
RUN                 echo 'eval "$(pyenv init -)"' >> ~/.zshrc
RUN                 echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc

# install python
ENV                 PATH /root/.pyenv/bin:$PATH
RUN                 pyenv install 3.6.5

# pipenv
RUN                 apt -y install software-properties-common python-software-properties
RUN                 add-apt-repository -y ppa:pypa/ppa
RUN                 apt -y update
RUN                 apt -y install pipenv

# Language
ENV                 LANG    C.UTF-8
ENV                 LC_ALL  C.UTF-8

RUN                 apt -y install vim

# Copy Pipfile
RUN                 mkdir /srv/project
COPY                ./Pipfile   /srv/project
WORKDIR             /srv/project
RUN                 pipenv install --skip-lock # 실행되는 시간을 줄이기 위해
```

```shell
docker run -t ec2-deploy:base -f Dockerfile.base
```

이제 Dockerfile.base를 바꾸는 경우는 Pipfile이 바뀌었을 경우에만 작성하면 된다.

그렇게 되면 기존에 있던 Dockerfile에는

```Dockerfile
FROM                ec2-deploy:base

# Copy project files
COPY                . /srv/project
WORKDIR             /srv/project

# Virtualenv path
RUN                 export VENV_PATH=$(pipenv --venv); echo $VENV_PATH;
ENV                 VENV_PATH $VENV_PATH

# Run uWSGI(CMD)
CMD                 pipenv run uwsgi \
                          --http :8000 \
                          --chdir /srv/project/app \
                          --home ${VENV_PATH} \
                          --module config.wsgi
```
Dockerfile.base를 기본으로 하며 프로젝트 파일만 복사한 뒤, uwsgi만 실행시키는 파일을 만들어놓았다.

```shell
docker build -t ec2-deploy -f Dockerfile .
```

위에서 home을 지정해주기 위해 pipenv --venv를 통해 나온 경로를 VENV_PATH에 할당하고 VENV_PATH를 Dockerfile로 실행하기 위해 ENV로 설정을 하고 ${}로 가지고 오면, Dockerfile에서 ENV로 지정한 값을 동적으로 가져다 넣을 수 있다.

왜냐하면 virtualenv가 어디에 있는지를 모르니까, 이렇게 home을 지정해주었다.

그런데 이때의 경로를 uwsgi.ini에서 동적으로 지정을 해줄수도 있다.

### uwsgi.ini

```ini
[uwsgi]
;파이썬 프로젝트로 change directory
chdir = ${PROJECT_DIR}/app
;가상환경 경로
;home = ${VENV_PATH}
;chdir로 바꾼 파이썬 프로젝트에서 wsgi모듈의 경로(path가 아닌 파이썬 모듈 경로)
module = config.wsgi:application

;http연결을 받으며, 8000번 포트로부터 받
http = :8000
```

### Dockerfile

```Dockerfile
FROM                ec2-deploy:base

ENV                 PROJECT_DIR       /srv/project

# Copy project files
COPY                . ${PROJECT_DIR}
WORKDIR             ${PROJECT_DIR}

# Virtualenv path
RUN                 export VENV_PATH=$(pipenv --venv); echo $VENV_PATH;

# Run uWSGI(CMD)
CMD                 pipenv run uwsgi --ini ${PROJECT_DIR}/.config/uwsgi.ini
```

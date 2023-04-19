---
layout: post
title: 세번째 프로젝트, 42world 배포과정 정리
category: PROJECT
tags: [PROJECT, 42world]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

## secret 키 분리 후

- .secrets/base.json
- settings 분리
- wsgi 분리
- settings, wsgi 각 경로 확인
- 런서버 확인

```
export DJANGO_SETTING_MODULE=config.setting.local
python manage.py runserver --settings=config.settings.local
```

## 배포

- aws EC2 생성 / 키페어 생성 / EC2 접속 / chmod 400 권한설정
  - `mv ~/Downloads/keypair_name.pem ~/.ssh`
  - `ssh -i ~/.ssh/keypair_name.pem ubuntu@<인스턴스 퍼블릭 DNS>`
  - `chmod 400 keypair_name.pem`

- IAM 유저 설정
  - AmazonEC2FullAccess권한을 가진 유저 추가

- EC2 인스턴스 ubuntu 기본 설정

```shell
# update, upgrade
sudo apt-get update
sudo apt-get dist-upgrade

# zsh 설치
sudo apt-get install zsh

# oh-my-zsh설치
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

sudo su
chsh ubuntu -s `which zsh`


# ~/.zshrc의 첫 번째 줄 주석 해제 (아래 부분)
export PATH=$HOME/bin:/usr/local/bin:$PATH

source ~/.zshrc

# pip 설치
sudo apt-get install python-pip
sudo python -m pip install -U pip

#pyenv 설치
curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash

# ~/.zshrc 맨 아래 추가
export PATH="/home/ubuntu/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"

# pyenv를 위한 requirements설치
# https://github.com/pyenv/pyenv/wiki/Common-build-problems
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev

# exit 후 ssh 재 접속 후 python 설치
pyenv install 3.7.4
```

정리하면서 추가중

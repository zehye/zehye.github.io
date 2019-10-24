---
layout: post
title: 세번째 프로젝트, 각종 에러들 해결하는 방법
category: PROJECT
tags: [PROJECT, 42world]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>


## Error: That port is already in use.

해당 포트가 이미 사용되고 있다는 의미로 사용되고 있는 포트 모두 종료시킨다.

`sudo lsof -t -i tcp:8000 | xargs kill -9`



## django.core.exceptions.ImproperlyConfigured: The SECRET_KEY setting must not be empty.

환경변수들을 분리해놓고 나면 자주보이는 에러이다.

```
vi ~/.zchrc

export DJANGO_SETTINGS_MODULE=config.settings.local
export DJANGO_SETTINGS_MODULE=config.settings.dev
export DJANGO_SETTINGS_MODULE=config.settings.production

source ~/.zchrc
```



## django.db.utils.OperationalError: (1045, "Access denied for user ''@'localhost' (using password: YES)")

mysql user 가 생성 안되어있나보다!

**brew통해 mysql 설치**

```
brew update
brew install mysql

mysql.server start
mysql_secure_installation
NO/YES/YES/YES/YES/YES

mysql -uroot -p
status  # uth8확인

exit
```

**user create**

```
mysql -uroot -p

create user '사용자'@'localhost' identified by '비밀번호';

grant all privileages on *.* to '사용자'@'localhost';  # 모든 DB에 접근가능
grant all privileages on DB이름.* to '사용자'@'localhost';  # 특정 DB에만 접근가능
```



## django.db.utils.OperationalError: (1049, "Unknown database 'django_local'")

해당 데이터베이스가 생성이 안되어있음으로 생성해주자


```
mysql> create database <database>
```



## Requirement.txt install error

```
pip install -r requirements.txt
```

**가상환경 셋팅확인**

```
pyenv vortualenv <가상환경 이름>
pyenv local <가상환경 이름>

# 해당 가상환경 내에서 pip install -r requirements.txt 실행
```

파이참 내부에서 하지않고 바깥 터미널에서 실행하기<br>
추가로 `pycharm interpreter` 확인

```
/usr/local/var/pyenv/versions/<파이썬 버전>/envs/<가상환경 이름>/bin/python3
```

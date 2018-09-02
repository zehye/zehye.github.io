---
layout: post
title: 첫번째 프로젝트, My real trip (django 설정)
category: PROJECT
tags: [project, my real trip]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## My real trip

```
project/
  trip-project/
    myrealtrip-solo

    git init
    .gitignore

    pyenv install django
    pyenv shell

    charm.
    pycharm interpreter 설정

    django-admin startproject config
    mv config app
    .manage.py runserver
```

### SECRET키 설정 제외하기

```
.secrets/
  base.json

app/
  config/
```

#### .secrets/base.json

```
{
  "SECRET_KEY": "-"
}
```

#### settings.py/base.py

```
BASE_DIR = os.path.dirname(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
ROOT_DIR = os.path.dirname(BASE_DIR)

SECRETS_DIR = os.path.join(ROOT_DIR, '.secrets')
secrets = json.load(open(os.path.join(SECRETS_DIR, 'base.json')))
SECRET_KEY = secrets['SECRET_KEY']
```

#### settings.py/local.py

```
from .base import *

DEBUG = True
ALLOWED_HOSTS = []

WSGI_APPLICATION = 'config.wsgi.application'

DATABASES = {
    'default': {
        'ENGINE': '~',
        'NAME': ~,
    }
}

STATIC_URL = '/static/'
``

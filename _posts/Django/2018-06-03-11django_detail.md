---
layout: post
title: Django - djangogirls-tutorial blog만들기 (+ 모듈 가져오기)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django 첫 시작에 대해 설명합니다.

<hr>

## 가상환경 설정

```
projects/
  django/
    djangogitls-tutorial/
      git init
      .git ignore(python, django, mac os, linux, git, pycharms+all)

      pyenv virtualenv 3.6.5 <가상환경 이름>
      pyenv local <가상환경 이름>

      - pycharm interpreter설정
      usr/loval/var/pyenv/versions/<가상환경 이름>/bin/python
```

### django 설치하기

```
pip install django~=1.11.0
```
우리는 django 버전을 1.11.0으로 설정했다.


### django 프로젝트 시작

```
django-admin startpoject mysite
```

의미는 mysite라는 django 프로젝트를 시작하겠다- 라는 의미이다.

즉, startpoject를 하는 동시에 mysite라는 폴더가 생성된다.
이때, django안에 mysite 폴더가 두개가 생기면서 헷갈릴 수 있으니

```
djangogitls-tutorial/
  mysite/ <- django프로젝트 관련 코드 컨테이너 폴더 이름
  manage.py
  mysite/ <-django 프로젝트의 설정 관련 패키지
    __init__.py
    settings.py
```

1.django코드 컨테이너 폴더의 이름을 app으로 변경
2.django프로젝트 설정 패키지의 이름을 config로 변경

```
djangogitls-tutorial/ <- 이 프로젝트의 컨테이너 폴더 (Root폴더)
  app/ <- django프로젝트 과년 코드 컨테이너 폴더
    config/ <- django 프로젝트의 설정 관련 패키지
      settings.py

  .gitignore <-django프로젝트(애플리케이션) 코드와 관계없지만 프로젝트를 위해 필요한 파일/폴더
  .git/
  requirements.txt
  ...
```

이후 app폴더로 들어가 blog폴더를 생성한다.

```
cd app
python manage.py startapp blog
```

그리고 config.setting로 들어가

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
]
```
`blog`를 추가한다.

이는 애플리케이션을 생성한 후 장고에 사용해야한다고 알려주는 과정이다.

### 모듈 가져오기

pycharm은 현재 프로젝트 구조에서 가장 상위폴더를 파이썬 source root로 인식한다.

그래서 지금은 `project` 폴더의 가장 상위 폴더를 root폴더로 인식하고 있는데, 우리는 `app`폴더를 root폴더로 인식할 수 있도록 변경해야한다. (일단 이 부분은 나중에)

```
project/
  project_module1.py
  project_module2.py
  pac1
    __init__.py
    pac1_module.py
      pac2
        __init__.py
        pac2_module.py
          pac3
            __init__.py
```

**python을 root에서 시작해서 project_module1을 가져올때**

import project_module1

**pac1패키지 내의 pac1_module을 가져올 때**

from pac1 import pac1_module

**pac2패키지 내의 pac2_module을 가져올 때**

from pac1.pac2 import pac2_module

### 하위 패키지에서의 동작
**pac2_module.py에서 pac2_module2.py를 가져오고 싶을 경우**

(pac2_module.py의 내용)
from         . import pac2_module2 (.은 현재 자신의 경로)

from pac1.pac2 import pac2_module2

**pac2_module.py에서 pac1_module.py를 가져오고 싶을 경우**

from   .. import pac1_module

from pac1 import pac1_module

**python을 pac1에서 시작했을 때 project_module1을 가져오려면**

불가능하다.




### requirements.txt만들기

현재 우리의 django 버전은 `1.11.0`인데 언제든 우리가 다양한 버전의 django를 하기 위해서는 requirements.txt를 만드는게 좋다.

```
pip list

Package        Version  
-------------- ---------
beautifulsoup4 4.6.0    
certifi        2018.4.16
chardet        3.0.4    
Django         1.11.13  
idna           2.6      
lxml           4.2.1    
pip            10.0.1   
pytz           2018.4   
requests       2.18.4   
setuptools     39.0.1   
urllib3        1.22  

pip freeze

beautifulsoup4==4.6.0
certifi==2018.4.16
chardet==3.0.4
Django==1.11.13
idna==2.6
lxml==4.2.1
pytz==2018.4
requests==2.18.4
urllib3==1.22
```

사실 django랑 pytz만 있으면 되는데, 나는 왜이렇게 많은건지?

```
pip freeze > requirements.txt
```

이렇게 하면 freeze에 있는 정보다 requirements.txt파일에 덮어씌워진다.

<hr>

```
git add -A
git commit

cd app
python manage.py runserver
```

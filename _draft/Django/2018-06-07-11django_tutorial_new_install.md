---
layout: post
title: Django - django-tutorial 1, 장고 최신버전으로 설정하기
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django-tutorial을 보고 정리했습니다.

<hr>
```
projects/
  django/
    new_django/
    git init
      .git
      .gitignore(python, django, git, macos, linux, pycharm+all)

      pyenv virtualenv 3.6.5 <가상환경 이름>
      pyenv local <가상환경이름>
      pycharm interpreter 설정 (usr/local/var/pyenv/versions/3.6.5/envs/<가상환경이름>/bin/python)
```

```
pip install django
django-admin startproject mysite

djangogitls-tutorial/
  mysite/ <- django프로젝트 관련 코드 컨테이너 폴더 이름 (app으로 변경 - 체크 x) / source root 설정
  manage.py
  mysite/ <-django 프로젝트의 설정 관련 패키지 (config로 변경 - 체크 o)
    __init__.py
    settings.py

pip freeze > requirements.txt
```

```
cd app
python manage.py startapp polls
```

```
projects/
  app/
    polls/
      __init__.py
      urls.py -> view function을 생성하면서 polls만의 urls.py를 만들어준다.
      admin.py
      apps.py
      migrations/
          __init__.py
      models.py
      tests.py
      views.py
```

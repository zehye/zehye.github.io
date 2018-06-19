---
layout: post
title: Django - django-tutorial, (models.py)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django-tutorial을 보고 정리했습니다.

<hr>

## models.py

```python
import datetime

from django.db import models
from django.utils import timezone

# 데이터베이스의 각 필드는 Field클래스의 인스턴스로서 표현된다.
class Question(models.Model):
  # CharField와 DateTimeField는 Question 클래스의 인스턴스이다.
  # CharField에는 max_length가 필수적으로 필요한 것처럼 Field클래스에는 필수인수가 필요하다.
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

    def __str__(self):
        return self.question_text

    def was_published_recently(self):
        """
        하루안에 발행된 포스트인가를 확인하는 메서드
        :return:
        """
        # 자신의 발행일자가 지금에서 하루만큼의 시간을 뺸 것
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)


class Choice(models.Model):

  # 각각의 Choice가 하나의 Question에 관계된다.
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

    def __str__(self):
        return self.choice_text
```

### settings.py

models.py를 만들고 나면 setting.py의 INSTALLED_APPS에 들어가

`polls.apps.PollsConfig`를 추가한다.

이는 `PollsConfig`클래스가 `polls/apps.py`에 있음을 의미하는 것이다.


### makemigrations, migrate

```python
python manage.py makemigrations polls
python manage.py migrate
```

`migration`은 모델의 변경사항을 저장하는 방법으로, 디스크상의 파일로 존재한다.

`polls/migrations/0001_initial.py`파일로 저장된 새 모델에 해단 migration을 읽어볼 수 있고 수동으로 django의 변경점을 저장하고 싶다면  사람이 직접 변경할 수 있도록 설계되어 있다.

`migration`을 하고 나면 자동으로 데이터베이스 스키마를 관리해주는 `migrate`를 해줘야한다.


### admin.py

```python
from django.contrib import admin
from .models import Question, Choice

admin.site.register(Question)
admif.site.register(Choice)
```

터미널로 돌아와

`python manage.py createsuperuser`설정을 하고

`python manage.py runserver`하면 admin 화면이 설정된다.

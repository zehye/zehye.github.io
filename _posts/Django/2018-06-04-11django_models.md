---
layout: post
title: Django 초기 설정하기1 (models.py)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django 첫 시작에 대해 설명합니다.

참조: https://tutorial.djangogirls.org/ko/django_models/
<hr>

```python
from django.utils import timezone
from django.conf import settings
from django.db import models

# Create your models here.

# models안에 있는 Model을 상속받는다.
class Post(models.Model):
    author = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
    )

    # 길이 제한 있는 text
    title = models.CharField(max_length=200)
    # 길이가 무한인 text
    text = models.TextField(blank=True)  
    # date와 time을 저장해놓은 필드
    created_date = models.DateTimeField(default=timezone.now)  
    published_date = models.DateTimeField(blank=True, null=True)

    # 나 자신의 발행일자에 현재 시작을 넣고(타임존 나우를 호출) 후 세이브 호출
    def publish(self):
        self.published_date = timezone.now()
        # 모델 안에 정의되어 있는 메서드
        self.save()

    # 객체를 문자열로 정의하고 싶을 때 사용
    def __str__(self):  
        return self.title
```

---
layout: post
title: Django - django-tutorial 4, (urls.py)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django-tutorial을 보고 정리했습니다.

<hr>

## urls.py

views.py에서 만든 함수와 사용자가 들어가서 볼 html파일을 연결한 url을 설정해준다.
app/polls/urls.py

```python
from django.urls import path
from . import views

# url의 이름공간 나누기(namespace)

# 실제 django의 project는 app이 몇개라도 올 수 있다.
# 이러한 수많은 app을 구분하기 위해 app의 namespace를 지정해준다.
app_name = 'polls'
urlpatterns = [
  path('', views.index, name = 'index'),
  path('<int:question_id>/', views.detail, name = 'detail'),
  path('<int:question_id>/results/', views.results, name = 'results'),
  path('<ing:question_id>/vote/', views.vote, name = 'vote'),
]
```

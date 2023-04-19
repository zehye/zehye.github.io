---
layout: post
title: Django - djangogirls-tutorial blog만들기 2, (models.py)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 djangogirls-tutorial을 보고 정리했습니다. 

참조: https://tutorial.djangogirls.org/ko/django_models/
<hr>

## python models.py
```
git checkout -b blog-model
```

```python
from django.utils import timezone
from django.conf import settings
from django.db import models

# Create your models here.

# models안에 있는 Model을 상속받는다.

# model을 정의하는 코드
class Post(models.Model):
    # models.ForeignKey : 다른 모델에 대한 링크
    author = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
    )

    # models.CharField : 글자수가 제한된 텍스트, 글 제목 같이 짧은 문자열
    title = models.CharField(max_length=200)
    # models.TextField : 글자수에 제한이 없는 긴 텍스트, 블로그 컨텐츠
    text = models.TextField(blank=True)
    # models.DateTimeField : 날짜오 시간
    create_date = models.DateTimeField(default=timezone.now)
    published_date = models.DateTimeField(blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```

그리고 데이터 베이스에 우리의 모델을 추가할 것이라고 명령을 내려야 하는데,
즉, 우리가 클래스를 작성했다는 것은 이 클래스를 가진 어떤 데이터베이스를 가졌으면 좋겠다이지 아직 만든건 아니니까
이제 이 모양으로 데이터베이스 테이블을 만들어줘라는 명령어를 내려줘야 한다.

### python makemigrations
```
cd app
python manage.py makemigrations

blog/migrations/0001_initial.py
  - Create model Post

python manage.py migrate
```

```
Operations to perform:
  Apply all migrations: admin, auth, blog, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
```

`migrations`: 어떤 변경사항에 대해 정의해놓은 파일들을 적용해놓은것

나머지는 자동으로 설정된것들 - installed된 애들의 정보 모든 것

데이터베이스에 대한 모든 기록들은 makemigration에 다 등록된다.

```
git checkout master
git merge blog-model

git checkout -d blog-model

git checkout -b admin
```

### python admin.py

admin.py에서
```
from django.contrib import admin
from .models import Post

admin.site.register(Post)

```

`python manage.py runserver` 후 `localhost:8000/admin`

shell에서
```
cd app

python manage.py createsuperuser 설정
```

비밀번호는 8자 이상으로 설정한다.

```
Username (leave blank to use 'jihye'): jihye
Email address:
Password:
Password (again):
Superuser created successfully.
```

그러고 로그인하면 성공!

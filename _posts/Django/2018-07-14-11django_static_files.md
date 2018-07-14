---
layout: post
title: Django - static files
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## db를 덮어씌우게 됐을 때의 문제점 2가지

앞서 deploy.sh에서 runserver를 실행했을때, 로컬에서 있던 내용을 덮어씌우는 것까지 했는데, 이는 문제를 일으킬 수 있는 요소가 많다. (주소도 너무 길고, :8000이 붙어있기도 하고)

근데 가장 문제중 하나는 db파일을 덮어쓰고 있다는 것이다. sqlite에서 쓰는 db를 쓰는게 아니라, 다른 저장소를 만들어 그곳의 데이터를 쓰는게 맞는 것같다.

그리고 우리가 사용하고 있는 EC2는 언제든지 없어질 수 있다는 가정하에 배포를 해야한다. 왜냐면 나중에 auto scaling이라는 것을 쓰게 되는데, 이는 1대의 컴퓨터에서 할 수 있는 처리에는 한계가 있기 때문에 이를 막기 위해 서버를 분산하는 것이다.

즉, 각각의 컴퓨터는 서로 의존적이지 않아야 하며 오로지 요청이 들어왔을 때 응답을 만들어주고 나머지 데이터는 바깥에 있어야 한다.

그리고 문제의 두번째는, 유저가 업로드 한 파일들은 db에 저장이 되는데, 이때의 db가 날라가 버리면 그냥 사라져버리는 것이다.

### settings.py/ views.py

```python
from django.shortcuts import render


def index(request):
    # TEMPLATE 설정에 app/templates폴더 추가
    # templates폴더에 'index.html' 추가

    # STATICFILES_DIRS에 app/static 폴더 추가
    # static폴더에 Bootstrap CSS파일(css/bootstrap.css)추가

    # index.html 에서 {% static %}태그를 사용해서
    # head태그 내의 link태그에 css/bootstrap.css를 불러오기

    # 임의의 이미지파일을 static/images 폴더에 추가
    # 해당 이미지파일을 {% static %}태그로 본문에 삽입

    # url '/'이 이 View와 연결되도록 urls.py설정
    return render(request, 'index.html')
```

### settings.py/ urls.py

```python
from django.conf import settings
from django.conf.urls.static import static
from django.contrib import admin
from django.urls import path
from .views import index

urlpatterns = [
    path('', index, name='index'),
    path('admin/', admin.site.urls),
] + static(
        prefix=settings.MEDIA_URL,
        document_root=settings.MEDIA_ROOT,
    )
```

### settings.py

```python
STATIC_DIR = os.path.join(BASE_DIR, 'static')

# Static
BASE_DIR = os.path.dirname(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
ROOT_DIR = os.path.dirname(BASE_DIR)
STATICFILES_DIRS = [
    STATIC_DIR,
]

STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(ROOT_DIR, '.static')

MEDIA_ROOT = os.path.join(ROOT_DIR, '.media')
MEDIA_URL = '/media/'
```

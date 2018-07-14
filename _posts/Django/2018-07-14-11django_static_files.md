---
layout: post
title: Django - 배포 환경에서 static files 설정하기 
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
# Static
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
STATIC_DIR = os.path.join(BASE_DIR, 'static')

STATICFILES_DIRS = [
    STATIC_DIR,
]

MEDIA_ROOT = os.path.join(ROOT_DIR, '.media')
MEDIA_URL = '/media/'
```
현재 우리가 만들어 놓은 static폴더에는 `css`, `images` 폴더가 각각 존재하고 장고 내부에서 admin안에 static, admin 폴더가 존재한다. 기본적으로 우리가 `static/~` 하고 파일을 불러올 경우 뒤에 나오는 파일의 이름은 겹치지 않도록 하게 되어있다. (둘다 참조하되, 무엇을 불러올지에 대한 혼란이 발생하기 때문)

그렇기 때문에 admin/statis/admin 이렇게 처음과 끝의 폴더의 이름을 같게 해주도록 되어있다.

우리가 url로 static이 넘어왔을때, 이를 참조하는 방법은 STATICFILES_DIRS에 무엇이 참조되고 있는지를 확인한 후 해당 파일을 불러온다. 그래서 이 경우에 이름이 겹치면 안되는 것이다.

우리가 어떤 어플리케이션을 쓸 지 모르고, css를 사용한다고 하면 우리가 만든 static/css를 쓸지 admin/static/admin/css을 쓸지에 대한 혼란을 막기 위해 위처럼 설정을 해주는 것이다.

우리가 파일을 runserver를 통해 불러오는 경우의 경로는

`browser -> runserver -> static/~ -> STATICFILES_DIRS에서 각각 검사 -> response`로 너무 많은 경로를 거쳐지나간다. (낭비되는 작업)

그런데 실제 배포과정에서는 요청을 받아주는 web server가 존재한다.

`browser -> web server -> static/~ -> django로 전달 or return`

여기서 web server는 외부에서 http 요청이 들어왔을 때, 정적파일을 돌려주는데에 가장 특화가 되어있다. (고정된 http, css image 등) 이러한 파일들은 web server에서 django까지 전달해줄 필요가 없이 바로 return해주면 된다.

그런데 만약 우리가 동적인 파일을 전달해줘야 할 경우, 렌더링된 html을 전달해주면되고(데이터베이스까지 왔다갔다 할 수 있는) 이는 web server가 할수가 없다. 장고 어플리케이션 안쪽에서 db와의 작업을 통해서 동적인 html 전달

이때, 렌더링이 필요없는 파일, 정적파일을 static파일이라고 하고 그대로를 돌려주면 되는데, 이를 돌려주는 과정은 장고를 안거쳐가는게 좋다. 성능상의 이득을 위해서 !

그래서 현재 STATICFILES_DIRS안에는 static파일들을 검색하는 경로가 여러개일 것이다. (지금도 현재 2개) 그런 이유로 이러한 static파일들을 모아준다 -> `collectstatic`

이 명령어를 실행하면 STATICFILES_DIRS에 있던 static폴더의 파일들을 하나로 모아주게 된다. 이런 모아진 파일을 웹 서버에서 static이라는 요청이 왔을때 이 폴더에서 가져가라고 명령하는 것이다. 즉, `static/~`하고 오는 요쳥은 장고까지 넘어가지 않고 웹 서버 단에서 곧바로 모아져있는 곳에서 가져가도록 한다. -> 그 곳이 `STATIC_ROOT`

그래서 같은 이름이 존재하면 안된다는 것이다.

### settings.py

```python
# Static
BASE_DIR = os.path.dirname(os.path.dirname(os.path.dirname(os.path.abspath(__file__)))))
ROOT_DIR=os.path.dirname(BASE_DIR)
STATIC_DIR = os.path.join(BASE_DIR, 'static')

STATICFILES_DIRS = [
    STATIC_DIR,
]

STATIC_URL='/static/'
STATIC_ROOT=os.path.join(ROOT_DIR, '.static')

MEDIA_ROOT = os.path.join(ROOT_DIR, '.media')
MEDIA_URL = '/media/'
```

여기서의 .static은 app폴더 바깥에 있는 `.static`폴더를 의미한다 (만들쟈)

그러고 나서
```python
./manage.py collectstatic
```

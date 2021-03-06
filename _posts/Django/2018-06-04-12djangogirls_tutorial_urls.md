---
layout: post
title: Django - djangogirls-tutorial blog만들기 3, (urls.py)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 djangogirls-tutorial을 보고 정리했습니다. 

<hr>

## urls.py 설정
config/ urls.py의 상태를 깨끗하게 유지해야한다.

> 지금 설정 폴더에 있는 (config안에 있는) urls.py는 urlresolver역할을 하는데, 해당 기능 함수가 늘어날 수록 폴더 안에 들어가는 애플리케이션이 굉장이 많아질 수 있다.
이때, 그 추가되는 애플리케이션을 config/urls.py에 모두 작성하게 되면 너무 많아지니까
> 각 해당하는 애플리케이션 내에 urls.py를 만들어 놓고 (urlresolver를 별도로 가지고 있는)
> config/urls.py 내에서는 각 서버에 연결시키기만 하는 역할을 하도록 한다.

```
config/urls

from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),

    # admin을 제외한 나머지는 blog.urls로 가도록 하라.
    # include뒤에 작성하는 것은 모듈명을 뜻하는 것, 즉 blog라는 패키지 안에 들어있는 urls 모델을 사용하라.
    # 여기서 include는 blog.urls로 그대로 들어가 blog.urls에 있는 정규표현식을 적용한다.
    url(r'', include('blog.urls'))
]
```

근데 우리는 아직 blog내에 urls.py를 만들지 않았으니까 `runserver`를 구동시켰던 shell에서 에러가 났을 것이다.

`ModuleNotFoundError: No module named 'blog.urls'`

그러니까 이제 blog 패키지 안에 urls.py를 만들어주자.

그러고 나서 runserver를 나가고 다시 들어가면 다시 다른 에러가 떠있다.

`ImproperlyConfigured` : 중간에 보면 이렇게 잘못된 설정 오류라고 뜰 것이다.

즉 우리가 설정한 'blog.urls'가 기존 projects내 경로를 통해서 가져오려고 했는데, 그 경로가 잘 못되었다는 의미로 blog.urls로 들어가 urlpattens를 설정해주자.

```python
urlpatterns = [

]
```
그러면 이제 runserver가 제대로 실행될 것이다.



### post-list

```python
from django.conf.urls import url
from .views import post_list
# from blog.views import post_list

urlpatterns = [
    # url의 첫번째 인자: 매치될 url 정규표현식
    # 매칭에 성공하면 두번째 인자로 넘어간다: view function
    # view function: request를 받아 response를 돌려주는 함수 -> view function을 만들어준다.(post-list)
    # blog.views에 있는 post_list함수를 아래 url함수의 두번째 인자로 전달 (함수호출 아님)

    # url은 무언가로 시작(^)하고 끝난다.($)
    url(r'^$', post_list),

]
```

## post_detail 추가된 뒤

```python
from django.conf.urls import url
from .views import post_list
from .views import post_detail
# from blog.views import post_list

urlpatterns = [
    url(r'^$', post_list),

    # 정규표현식에 그룹을 지정해서 view function의 인수로 전달한다.

    # url은 숫자(\d)로 시작(^)하고 1개 이상 최대 존재(+)하고 끝난다 (/)
    url(r'^(\d+)/', post_detail),
]
```

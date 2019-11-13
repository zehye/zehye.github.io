---
layout: post
title: 세번째 프로젝트, google social login
category: PROJECT
tags: [PROJECT, 42world]
comments: true
---

> 개인 프로젝트를 정리한 것입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

`pip install django-allauth`

## settings/base.py

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.sites', # 추가
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # 추가
    'allauth',
    'allauth.account',
    'allauth.socialaccount',

    'allauth.socialaccount.providers.google',
]
```

그리고 맨 아래줄에 다음 문구 추가

```
AUTHENTICATION_BACKENDS = (
    'django.contrib.auth.backends.ModelBackend',
    'allauth.account.auth_backends.AuthenticationBackend',
)

SITE_ID = 1
LOGIN_REDIRECT_URL = '/'
```


## config/urls.py

```
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index),
    path('accounts/', include('allauth.urls')), # 추가
]
```

```
./manage.py migrate
```

runserver 실행 후 admin 페이지로 들어가기

```
localhost:8000/admin
```

### SITES

SITES 를 클릭하면 `example.com`이 있을텐데 해당 문구 클릭해 들어가서 `http://localhost:8000`으로 변경 후 저장


### Social application

- providers: google
- name: application name
- Client id: 구글에서 제공하는 클라이언트 ID
- Secret key: 구글에서 제공하는 Secret Key
- Sites: Choose all 클릭

[구글에서 값 가져오기](https://console.developers.google.com/apis/)

- 새 프로젝트 생성
- 사용자 인증 정보 > OAuth 클라이언트 ID 클릭
- 어플리케이션 유형 > 웹 어플리케이션 선택
- 승인된 자바스크립트 원본 > http://localhost:8000
- 승인된 리디렉션 URI 아래 주소 저장
  - http://localhost:8000
  - http://localhost:8000/accounts/google/login/callback/


## index.html

구글 로그인이 잘 되는지 확인해보기 위해 간단한 html 구현을 해본다.

```html
{% raw %}
<!doctype html>
<body>
{% load socialaccount %}
{% providers_media_js %}
<h1>소셜 로그인을 해봐요!</h1>
<a href = "/accounts/signup">회원가입</a><br>
{% if user.is_authenticated %} <a href = "/accounts/logout">로그아웃</a>
{{user.username}} 님이 로그인중
{% else %}
<a href=" {% provider_login_url 'google' %}">구글 로그인</a><br>
로그인 하셔야 합니다.
{% endif %}
</body>
</html>
{% endraw %}
```

### config/urls.py

```
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index), # 추가
    path('accounts/', include('allauth.urls')),
]
```

### config/views.py

```python

def index(request):
  return render(request, 'index.html')
```

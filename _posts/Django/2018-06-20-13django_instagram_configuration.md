---
layout: post
title: Django - Instagram만들기, pipenv설치(~ MEDIA ROOT, URL까지)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django-document을 보고 정리했습니다.

<hr>

### pipenv 설치하기

`brew install pipenv`

pipenv --python 3.6.5
<pyenv virtualenv 3.6.5하던것>
pipenv install django

git init
vi .gitignore
pipenv shell
django-admin startproject mysite

interpreter설정 잊지말기

Users/name/.virtualenv/foldername/bin/python

MEDIA_ROOT설정하기
admin으로 들어가서 post를 생성했는데, 이미지를 누르면 이미지가 나오지 않는다.
그러고 파이참에 들어가면 새로운 폴더 하나가 생긴다. (post -> model에서 upload_to가 post여서)
애플리케이션과 이름이 같으면 너무 헷갈리니까, 특별한 곳으로 옮기고 싶다.
static dir or template dir를 설정했던 것처럼

얘네도 정적파일인데, 이 정적파일은
프로젝트에 포함된 스테틱 파일이랑, 유저 업로드 파일 두개로 나뉜다.
유저 업로드 파일은 미디어 파일이라고도 부른다.


만약에 유저가 업로드한 파일을 우리가 개발과정에서 보고싶다면?

처음에는 url패턴만 받고 뒤에는 url패턴에서 파일을 찾을 경로를 받는다.
MEIDA_ROOT 실제 경로를 정하는것, 실제경로
MEDIA_URL 사용자가 올린 파일들을 어떤 url 경로를 기준으로 제공을 할 것인지

app/media를 만들고 settings.py에서 MEDIA_ROOT를 설정해준다.
그 상태에서 admin으로 보면 사진의 경로가 이상하게 나타나는데, 제대로 된 경로를 보여주려면
settings.py 맨 아래, '/media/'를 만들어줘야 한다.
이는 User-uploaded file을 접근할 URl접두어이다.
접두어는 이렇게 시작되면 뒤에 뭐가 오든간에 앞에 이렇게 오면 무조건 정적 파일 가져오는 것으로 취급하겠다는 것.

그렇게 나타난 사진을 누르면 실제 뜨게 하기 위해서 (page not found error를 피하려면)

config의 urls.py를 수정해줘야 한다.
```python
urlpatterns = [
    path('', views.index, name='index'),
    path('admin/', admin.site.urls),
    path('posts/', include('posts.urls'))
] + static(
    #prefix는 접두어
    prefix='/media/',
    document_root=settings.MEDIA_ROOT,
)
```

post_list.html에서 처음 {{ post.photo }}를 하면 단순 문자열이 나온다.
템플릿을 렌더링하면 객체가 나오는게 아니라 객체를 문자열로 변환을 해준다.
html이 알아들을 수 있는 것은 문자열이나 태그밖에 없으니까.

그렇게 해서 출력된 문자열은 db browser를 통해서 어디에 포함되어 있는지를 확인해보면
posts_post에 문자열 그대로 들어가 있고 그대로출력이 된 것이다.

우리가 config/url에서 media url 경로를 설정해줬던 그대로 출력되어있다.
이 이미지를 그대로 img태그를 통해 넣어주면 해당 파일이 나온다.
<img src="/media/ {{ post.photo }}" alt="">
이를 좀더 간단하게 표현하면
/media/를 쓰면 된다는 것은 알지만 지금 이건 하드코딩이니까, 이 내용은 우리가 settings.py에서 동적으로 바꿀 수 있다.



```python
urlpatterns = [
    path('', views.index),
    path('admin/', admin.site.urls),
    path('posts/', include('posts.urls'))
] + static(
    prefix=settings.MEDIA_URL,
    document_root=settings.MEDIA_ROOT,
)
```
이렇게 만들었을 때, media라는 글자가 나오면 무조건 등장할 수 있도록 해준다.
<img src="{{ post.photo.url }}" alt="">
이 url 속성은 이미지필드, 파일 필드에 대해서 자동으로 url을 만들어 주는 것이다.

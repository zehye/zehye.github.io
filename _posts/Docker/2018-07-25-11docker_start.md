---
layout: post
title: Docker 프로젝트 초기설정
category: Docker
tags: [docker]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## 프로젝트 초기 설정
```
project/deploy/eb-docker-deploy
git init
cp ../ec2-deploy/.gitignore .

pipenv install django
pipenv shell

django-admin startproject config
mv config app
./manage.py runserver
```

그리고 first commit을 하기 전에 `SECRET KEY`를 빼준다.
```
charm .
interpreter설정
.secrets/base.json
settings -> settings/base.py -> base.json으로부터 SECRET_KEY할당
            settings/local.py
```

### .secrets/base.json

```json
{
  "SECRET_KEY": "settings.py에 있는 SECRET_KEY"
}
```

### settings.py/base.py 수정사항

```python
BASE_DIR = os.path.dirname(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
ROOT_DIR = os.path.dirname(BASE_DIR)
SERCRETS_DIR = os.path.join(ROOT_DIR, '.secrets')
sercrets = json.load(open(os.path.join(SERCRETS_DIR, 'base.json')))

SECRET_KEY = secrets['SECRET_KEY']
```

### settings.py/local.py 추가할 부분

```python
from .base import *

DEBUG = True
ALLOWED_HOSTS = []

WSGI_APPLICATION = 'config.wsgi.application'

DATABASES = {
    'default': {
        'ENGINE': '~',
        'NAME': ~,
    }
}

STATIC_URL = '/static/'
```

이제는 EXPORT djnago 세팅을 해야 runserver가 작동된다는 걸 알 수 있다.

커밋을 남기기 전에, `git status`를 통해 ` .secrets`폴더가 포함되지 않았는지를 확인한다. 만약 포함이 되어있다면 `gitignore`를 통해 빼주도록 한다.

<hr>
### github으로 가서

```
settings/ SSH and GPG keys
```
공개키를 서버에 등록하는 곳으로, 공개키를 이곳에 등록을 해놓으면 git에 어떤 내용을 push하고 싶다고 보낼때 어떤 사용자라는 것을 공개키를 알고잇으니 공개키를 통해 우리에게 암호화된 데이터를 보낸다.

지문이라는 기법을 통해 공개키로 결국 암호화된 데이터를 보내면, 이를 해석할 수 있는 것은 우리밖에 없다. 키가 만약 개인키가 우리에게 있고 공개키를 저장했다고 했을때, 공개키로 암호화해서 보내면 이를 해석할 수 있는건 나뿐이다.

저쪽에서 공개키로 압축해서 암호화해서 보내면 해석은 나만이 가능하다.

비밀번호를 안치고, 상대가 보낸 응답의 해석을 우리가 보낼 수 있으면, 내가 개인키를 가지고 있으니 사용자라는 걸 알 수 있다.

따라서 SSH키를 만들어야 한다.

```shell
cd ~/.ssh
ssh-keygen -C 'github 이메일주소'

Generating public/private rsa key pair.
Enter file in shich to save the key (/Users/jihye/.ssh/id_rsa):
# 그냥 앞으로 나오는 질문에는 엔터만 세번 눌러주면 완성
```

이제 `id_rsa.pub`의 키를 github에 등록해준다.

대부분의 title은 자신이 사용하는 노트북의 이름을 써준다.

그리고 저장소를 생성할 때 ssh로 생성하면서 pycharm에서

```shell
git remote add origin git@github.com:<~>
git push -u origin master
```

### 커스텀 유저 만들기

```shell
./manage.py startapp members
```

#### members/models.py

```python
class User(AbstractUser):
  pass
```

```
./manage.py makemigration
```

#### members/admin.py

```python
from django.contrib.auth.admin import UserAdmin as BaseUserAdmin
from .models import User

class UserAdmin(BaseUserAdmin):
  pass

admin.site.register(User, UserAdmin)
```

#### config/settings/base.py

```shell
AUTH_USER_MODEL = 'members.User'
INSTALLED_APPS = [
  'members',
]
```

추가

```shell
gaa
gc -m 'Custom UserModel'
```

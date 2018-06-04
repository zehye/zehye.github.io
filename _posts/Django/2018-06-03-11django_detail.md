---
layout: post
title: Django 가상환경 설정 (+ 모듈 가져오기)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django 첫 시작에 대해 설명합니다.

<hr>

## 가상환경 설정

```
projects/
  django/
    djangogitls-tutorial/
      git init
      .git ignore(python, django, mac os, linux, git, pycharms+all)

      pyenv virtualenv 3.6.5 <가상환경 이름>
      pyenv local <가상환경 이름>

      - pycharm interpreter설정
      usr/loval/var/pyenv/versions/<가상환경 이름>/bin/python
```

### django 설치하기

```
pip install django~=1.11.0
```
우리는 django 버전을 1.11.0으로 설정했다.


### django 프로젝트 시작

```
django-admin startpoject mysite
```

의미는 mysite라는 django 프로젝트를 시작하겠다- 라는 의미이다.

즉, startpoject를 하는 동시에 mysite라는 폴더가 생성된다.
이때, django안에 mysite 폴더가 두개가 생기면서 헷갈릴 수 있으니

```
djangogitls-tutorial/
  mysite/ <- django프로젝트 관련 코드 컨테이너 폴더 이름
  manage.py
  mysite/ <-django 프로젝트의 설정 관련 패키지
    __init__.py
    settings.py
```

1.django코드 컨테이너 폴더의 이름을 app으로 변경
2.django프로젝트 설정 패키지의 이름을 config로 변경

```
djangogitls-tutorial/ <- 이 프로젝트의 컨테이너 폴더 (Root폴더)
  app/ <- django프로젝트 과년 코드 컨테이너 폴더
    config/ <- django 프로젝트의 설정 관련 패키지
      settings.py

  .gitignore <-django프로젝트(애플리케이션) 코드와 관계없지만 프로젝트를 위해 필요한 파일/폴더
  .git/
  requirements.txt
  ...
```

### 모듈 가져오기

pycharm은 현재 프로젝트 구조에서 가장 상위폴더를 파이썬 source root로 인식한다.

```
project/
  project_module1.py
  project_module2.py
  pac1
    __init__.py
    pac1_module.py
      pac2
        __init__.py
        pac2_module.py
          pac3
            __init__.py
```

**python을 root에서 시작해서 project_module1을 가져올때**

import project_module1

**pac1패키지 내의 pac1_module을 가져올 때**

from pac1 import pac1_module

**pac2패키지 내의 pac2_module을 가져올 때**

from pac1.pac2 import pac2_module

### 하위 패키지에서의 동작
**pac2_module.py에서 pac2_module2.py를 가져오고 싶을 경우**

(pac2_module.py의 내용)
from         . import pac2_module2 (.은 현재 자신의 경로)

from pac1.pac2 import pac2_module2

**pac2_module.py에서 pac1_module.py를 가져오고 싶을 경우**

from   .. import pac1_module

from pac1 import pac1_module

**python을 pac1에서 시작했을 때 project_module1을 가져오려면**

불가능하다.


<hr>
djangogitls-tutorial에서 view.py파일에서 def post_list함수의 경로 설정

```python
"""
first/
    first_file.txt
    second/
        second_file.txt
        third/
            module.py
            fourth/
                fourth_file.txt

modele.py에서
0. 현재 경로
    os.path.abspath(__file__)
1. third/ 폴더의 경로
    os.path.dirname(<현재경로>)
1-1. second/ 폴더의 경로
    os.path.dirname(<third폴더의 경로>)
2. second/second_file.txt의 경로
    os.path.join(<second폴더의 경로>, 'second_file.txt')
3. fourth/ 폴더의 경로
    os.path.join(<현재경로>, 'fourth')
4. fourth/fourth_file.txt의 경로
    os.path.join(<현재경로>, 'fourth', 'fourth_file.txt')

-> def post_list에서 templates/blog/post_list.html파일의 내용을 읽어서 return 해주는 HttpResponse에 전달
:param request:
:return:
"""
cur_file_path = os.path.abspath(__file__)
# print(cur_file_path)
third_file_path = os.path.dirname(cur_file_path)
# print(third_file_path)
second_file_path = os.path.dirname(third_file_path)
# print(second_file_path)
second_second_file_path = os.path.join(second_file_path, 'templates')
# print(second_second_file_path)
fourth_fourth_file_path = os.path.join(second_second_file_path, 'blog', 'post_list.html')
print(fourth_fourth_file_path)

# html = open(fourth_fourth_file_path, 'rt').read()
#
# 경로에 해당하는 html파일을 문자열로 로드해줌
# render_to_string : (path) -> template dir를 기준으로 가져온 특정 path값을 가져온다.
#
# 근데 이때의 pathsms setiings.py의 TEMPLATES안에 있는 path를 기준으로 해서 가져온다.
# 가져온 문자열 돌려주기
html = render_to_string('blog/post_list.html')

# 특정 리퀘스트가 올떄 보통 http로 오고 여기로 응답을 보내는데 응답을 보내기 위한 무언가를 만들어준다.
return HttpResponse(html)  

# 위의 두 줄을 한번에 줄여쓰는 방법
return render(request, 'blog/post_list.html')
```

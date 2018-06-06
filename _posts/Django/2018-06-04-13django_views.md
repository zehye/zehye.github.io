---
layout: post
title: Django - djangogirls-tutorial blog만들기 4, (views.py)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django 첫 시작에 대해 설명합니다.

<hr>

## post_list view function

djangogitls-tutorial에서 view.py파일에서 def post_list함수의 경로 설정

```python
import os

from django.http import HttpResponse
from django.shortcuts import render
from django.template.loader import render_to_string

from .models import Post


# view function은 항상 첫번째 인자로 request를 갖는다.
def post_list(request):
  # return HttpResponse('Post list입니다.')
  # 여기서 HttpResponse는 보통 우리가 보내는 모든 요청은 Http형식이고 이 Http는 응답을 보낸다.
  # 이때 HttpResponse는 Http응답을 보내기 위한 어떤 요청을 만들어낸다.

  # 즉 얘네도 결국 class이고, 이 class의 인스턴스 뒤에 문자열을 보내면 ('Post list~')
  # 이 문자열을 Http형태로 보내준다. (Http응답을 보내준다.)

  # 더 복잡한 연결을 하기 위해 app/templates/post_list.html을 만든다.



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

  html = open(fourth_fourth_file_path, 'rt').read()
  # 근데 매번 이렇게 해주는게 너무 번거로우니, settings.py에 TEMPLATES_DIR를 설정해준다.

  # 경로에 해당하는 html파일을 문자열로 로드해줌
  render_to_string : (path) -> template dir를 기준으로 가져온 특정 path값을 가져온다.

  # 근데 이때의 path는 setiings.py의 TEMPLATES안에 있는 path를 기준으로 해서 가져온다.
  # 가져온 문자열 돌려주기
  html = render_to_string('blog/post_list.html')

  # 특정 리퀘스트가 올떄 보통 http로 오고 여기로 응답을 보내는데 응답을 보내기 위한 무언가를 만들어준다.
  # 이렇게만 하면 Templates does not exist error가 나온다.

  # 왜냐하면 우리가 settings.py에 TEMPLATES_DIR를 만들어줬지만 얘가 어디를 참조해야 하는 지는 안 알려줬기 때문이다.
  # 그래서 다시 settings.py로 가서 설정을 해준다.
  return HttpResponse(html)  

  # 위의 두 줄을 한번에 줄여쓰는 방법
  # 결국 이 한줄만 사용하면 된다.
  return render(request, 'blog/post_list.html')
```
```python
def post_list(request):
  posts = Post.objects.all()
  context = {
    'posts': posts, }

    # render는 주어진 인수를 사용해서
    # 여기서 1번째 인수: HttpResponse인스턴스
    # 2번째 인수: 문자열 (TEMPLATES['DIRS']를 기준으로 탐색할 템플릿 파일의 경로
    # 3번째 인수: 템플릿을 렌더링 할 때 사용할 객체의 모음
    return render(request, 'blog/post_list.html', context)

    # 여기서 render는 django.shortcuts 패키지에 있는 함수로서 첫번째 파라미터로 request를 받는다.
    return render(
        request=request,
        template_name='blog/post_list.html',
        context=context
    )
    # 둘 중 하나를 쓰면 된다.
```

### post_detail view function

```python
def post_detail(request, post_id):
    post = Post.objects.get(id=post_id)
    context = {
        'post':post
    }
    # return HttpResponse(post_id)

    # 숙제: post_detail view function이 올바르게 동작하는 html을 작성해서 결과보기
    return render(request, 'blog/post_detail.html', context)
```

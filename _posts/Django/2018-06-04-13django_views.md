---
layout: post
title: Django 초기 설정하기3 (views.py)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django 첫 시작에 대해 설명합니다.

<hr>

```python
import os

from django.http import HttpResponse
from django.shortcuts import render
from django.template.loader import render_to_string

from .models import Post


def post_list(request):
  posts = Post.objects.all()
  context = {
    'posts': posts, }

    # render는 주어진 인수를 사용해서
    # 여기서 1번째 인수: HttpResponse인스턴스
    # 2번째 인수: 문자열 (TEMPLATES['DIRS']를 기준으로 탐색할 템플릿 파일의 경로
    # 3번째 인수: 템플릿을 렌더링 할 때 사용할 객체의 모음
    # return render(request, 'blog/post_list.html', context)
    return render(
        request=request,
        template_name='blog/post_list.html',
        context=context
    )

```

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

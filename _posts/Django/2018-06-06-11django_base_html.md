---
layout: post
title: Django - djangogirls-tutorial blog만들기 7, (base_html)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django 블로그 만들기에 대해 설명합니다.

<hr>

```html
{% raw %}
{% load static %}
<!--<!doctype html>-->
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>My First Blog</title>
    <link rel="stylesheet" href="{% static 'css/bootstrap.css' %}">
</head>
<body>
    <div class="container">
        <!--아래 텍스트를 클릭 시 무조건 post_list view function에 해당하는 페이지로 이동-->
        <!--여기서는 받아줘야할 인수가 없으니까 단순히 url에서 이름만 정해주면 된다 -->
        <h1 class="mb-4"><a href="{% url 'post-list' %}">DjangoGirls Blog</a></h1>
        <div class="text-right mb-3">
            <!--<button class="btn btn-primary">글 쓰기</button>-->
            <a class="btn btn-primary" href="{% url 'post-create' %}">글 쓰기</a>
        </div>
        {% block content %}
        {% endblock %}
        {% endraw %}
    </div>
</body>
</html>
```

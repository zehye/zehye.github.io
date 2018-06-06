---
layout: post
title: Django - djangogirls-tutorial blog만들기 10, (post_create.html)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django 첫 시작에 대해 설명합니다.

<hr>

```html
<!--1. 템플릿 작성-->
<!--blog/base.html을 기반으로 content block에 -->
<!--form요소를 작성-->
    <!--form요소는 내부에 input, textarea, button를 하나씩 가짐-->

<!--2. 이 템플릿을 post_create() view function에서 render함수를 사용해 처리된 결과 돌려주기-->

<!--3. post_create() view function와 blog.urls를 매칭시키기-->
<!--매칭되는 url은 -->
<!--3-1 정규표현식-->
<!--localhost:8000/write/ <- 이 url에 해당되도록 한다.-->
<!--3-2.url의 이름-->
<!--post-create라는 이름을 붙인다.-->
<!--3-3. base.html에서 글쓰기 링크 만들기-->
<!--blog/base.html의 '글 쓰기' a태그의 href속성에서-->
<!--post_create() view function에 해당하는 위치로의 링크를 url 태그를 사용해 생성한다.-->

{% extends 'blog/base.html' %}
{% block content %}
<!--action이 비어있으면 현재위치(url)로 보여준다-->
<form action=""  method="POST">
    {% csrf_token %}
    <div class="formgroup">
        <label for="title">제목</label>
        <input name="title" type="text" id="title" class="form-control" placeholder="제목을 입력하세요">
    </div>
    <div class="formgroup">
        <label for="text">내용</label>
        <textarea name="text" id="text" class="form-control" cols="30" rows="10"></textarea>
    </div>
    <div class="formgroup text-right mb-4">
        <button type="submit" class="btn btn-primary" >제출</button>
    </div>
</form>
{% endblock %}
```

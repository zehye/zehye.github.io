---
layout: post
title: Django - djangogirls-tutorial blog만들기 8, (post_list.html)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 djangogirls-tutorial을 보고 정리했습니다. 

<hr>

```html
{% raw %}
{% extends 'blog/base.html' %}
{% block content %}
    {% for post in posts %}
    <div class="card mb-4">
        <div class="card-body">
            <!--post_detail로 이동할 수 있도록 -->
            <!--a태그의 href속성을 동적 저장 -->
            <!--url에서 만든 tag name의 post-detail과 같은 이름을 의미한다 -->
            <!---->
            <!--post-detail이라는 이름을 가지는 urls.py의 url에 하나의 인수(post.id)를 사용해서-->
            <!--해당하는 url을 만들어준다-->
            <h5 class="card-title"><a href="{% url 'post-detail' post.id %}">[{{ post.id }}] {{ post.title }}</a></h5>
            <h6 class="card-subtitle text-muted mb-3 text-right">published: {{ post.published_date }}</h6>
            <p class="card-text">{{ post.text|linebreaksbr|truncatechars:100 }}</p>
            {% comment %}
            줄 바꿈이 있을 때마다 br태그를 알아서 넣어줘 -> |linebreaksbr
            100자까지만 보여줘 -> |truncatechars:100
            {% endcomment %}

            <div>
                <form action="{% url 'post-delete' post.id %}" method="POST">
                    {% csrf_token %}
                    <button type="submit" class="btn btn-sm btn-danger">삭제</button>
                </form>
            </div>
        </div>
    </div>
    {% endfor %}
{% endblock %}
{% endraw %}
```

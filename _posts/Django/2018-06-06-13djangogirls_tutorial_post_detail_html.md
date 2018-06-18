---
layout: post
title: Django - djangogirls-tutorial blog만들기 9, (post_detail.html)
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
    <div class="card mb-4">
        <div class="card-body">
            <h5 class="card-title">[{{ post.id }}] {{ post.title }}</h5>
            <h6 class="card-subtitle text-muted mb-3 text-right">published: {{ post.published_date }}</h6>
            <p class="card-text">{{ post.text|linebreaksbr }}</p>
        </div>
    </div>
{% endblock %}
{% endraw %}
```

---
layout: post
title: Django - django-document 5, introduction to models (Model Inheritance)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django-document을 보고 정리했습니다.

<hr>

### Model Inheritance

`django`의 모델 상속은 파이썬에서 일반적인 클래스 상속이 작동하는 방식과 거의 동일하게 작동하지만, 반드시 따라야 하는 기본사항이 있다. 기본 클래스가 `django.db.models.Model`을 상속받아야 한다.

부모 모델이 자체 데이터베이스 테이블을 가지는 모델이 될지, 또는 부모가 자식 모델에게 전달할 정보만을 가지고 있는 지 여부만 결정하면 된다.

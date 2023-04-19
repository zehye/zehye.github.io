---
layout: post
title: Django - django-document 4, introduction to models (Model Attribute, Model Method)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django-document을 보고 정리했습니다.

<hr>

### Model Attribute

#### objects

모델 클래스에서 가장 중요한 속성은 `Manager`이다. `Maneger`객체는 모델 클래스를 기반으로 데이터베이스에 대한 쿼리 인터페이스(ORM)를 제공하며, 데이터베이스 레코드를 모델 객체로 인스턴스화 하는 데 사용된다.

특별히 `Manager`를 할당하지 않으면 장고는 기본 Manager를 클래스 속성으로 자동 할당시킨다. 이때 속성의 이름이 objects이다.

Manager는 모델 클래스를 통해 접근할 수 있으며, 모델 인스턴스(객체)를 통해서는 접근할 수 없다.


### Model Method

모델 객체(row)단위의 기능을 구현하려면 모델 클래스에 메서드를 구현해주면 된다.

테이블단위의 기능은 Manager에 구현한다.

이러한 규칙은 비즈니스 로직을 모델에서 관리하는데 있어 중요한 테크닉인데, 예를 들어, 다음과 같이 커스텀 메서드를 추가할 수 있다.

```python
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    birth_date = models.DateField()

    def baby_boomer_status(self):
        "Returns the person's baby-boomer status."
        import datetime
        if self.birth_date < datetime.date(1945, 8, 1):
            return "Pre-boomer"
        elif self.birth_date < datetime.date(1965, 1, 1):
            return "Baby boomer"
        else:
            return "Post-boomer"

    def _get_full_name(self):
        "Returns the person's full name."
        return '%s %s' % (self.first_name, self.last_name)
    full_name = property(_get_full_name)
```
이 예제에서, 마지막 메서드는 property이다.

**__str__()**

모델 객체가 문자열로 표현되어야 하는 경우에 호출된다. admin이나 console에서 많이 쓰인다.

기본 구현은 아무 도움이 되지 않는 문자열을 리턴하기 때문에, 모든 모델에 대해 오버라이드 해서 알맞게 구현해주는게 좋다.

#### overriding predefined model method

이미 정의되어 있는 모델 메서드를 새로 정의한다는 뜻으로 부모의 함수를 자식에서 재 정의하는 것을 의미한다.

슈퍼 클래스 메서드를 호출하는 것을 기억하자.

> super().save (* args, ** kwargs) 를 말합니다.

이는 객체가 데이터베이스에 저장 되도록 하는데,
수퍼 클래스 메서드를 호출하는 것을 잊어 버리면 기본 동작히 실행되지 않으며 데이터베이스에 저장하지 않는다.

또한 모델 메서드에 전달할 수있는 인수를 전달하는 것이 중요한데, 이는 `*args`와 `**kwargs`두 변수가 수행한다.

Django는 수시로 내장 모델 메서드의 기능을 확장하여 새로운 인수를 추가하고, 메서드 정의에서 `*args`, `**kwargs`를 사용하면 코드가 추가 될 때 해당 인수를 자동으로 지원한다는 보장을 받는다.

> 오버라이드된 모델 메서드는 `bulk operations`에서는 동작하지 않는다.
> QuerySet을 사용하여 대량으로 객체를 삭제할 때 또는 계단식 삭제의 결과로 객체의 delete() 메서드가 반드시 호출되지 않을 수 있다. 사용자 정의 삭제 논리를 실행하려면 `pre_delete`와 `post_delete` 시그널을 사용할 수 있습니다.
> 불행하게도 `bulk operations`에서는 `save()`메서드와 `pre_save`및 `post_save` 시그널이 호출되지 않기 때문에 객체를 대량으로 만들거나 업데이트 할 때 해결 방법이 없습니다.

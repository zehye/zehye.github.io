---
layout: post
title: Django - django-document 2, introduction to models (relationships부터)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django-document을 보고 정리했습니다.

<hr>

## models

### Relationships

관계형 데이터베이스의 강력함은 테이블간의 관계에 있다. 장고는 데이터베이스의 관계 유형 중 가장 일반적인 3가지를 제공한다.

`ManyToOneField`, `ManyToManyField`, `OneToOneField`

#### ManyToOneField

Many-to-one관계를 정의하기 위해서는 `ForeignKey`를 사용한다.

`ForeignKey`는 관계를 정의할 모델 클래스를 인수로 가져야 한다.

예로 들어 `Car`모델은 `Manufacturer`를 `ForeignKey`로 갖는다. `Manufacturer`는 여러개의 `Car`를 가질 수 있지만, `Car`는 오직 하나의 `Manufacturer`만을 갖는다.

```python
from django.db import models


class Car(models.Model):
  # ForeignKey필드의 이름은 해당 모델의 lowercase를 추천하지만 필수는 아니다.
    manufacturer = models.ForeignKey(
        # 원래는 class Manufacturer이 위에 있었는데 아래로 내려가면 참조를 하지 못한다.
        # 이때는 Manufacturer를 문자열로 설정을 하면 일단 이부분은 넘어가고
        # 실제 Manufacturer을 사용할때 아래를 참조할 수 있게 된다.
        'Manufacturer',

        # 만약에 제조사가 삭제되면 모든 차가 다 삭제된다.
        on_delete=models.CASCADE,
    )

    name = models.CharField(max_length=50)

    def __str__(self):
        return self.name


class Manufacturer(models.Model):
    name = models.CharField(max_length=50)

    def __str__(self):
        return self.name
```

**apps.py**

```python
from django.apps import AppConfig


class ForeignkeyConfig(AppConfig):
    name = 'models.foreignkey'
```

`makemigrations`, `migrate`하기

ipython shell에서

```python

hyundai = Manufacturer.objects.create(name='현대')
car1 = Car.objects.create(manufacturer=hyundai, name='아반떼')
car2 = Car.objects.create(manufacturer=hyundai, name='그렌져')
car3 = Car.objects.create(manufacturer=hyundai, name='소나타')

car1
>>> <Car:아반떼>
car1.manufacturer
>>> <Manufacturer:현대>

# Manufacturer의 Car목록을 보는 방법
Manufacturer.car_set.all()
>>> <QuerySet [<Car: 아반떼>, <Car: 그렌저>, <Car: 에쿠스>]>
```

models.py에

```python
class ForeignKeyUser(models.Model):
    name = models.CharField(max_length=50)
    instructor = models.ForeignKey(
        # 자기 자신을 참조
        # 누군가 다른 유저인데, 다른 유저를 자기 자신의 강사로 지정할 수 있다.
        'self',
        # 강사가 삭제가 되면 자신의 강사 항목은 null로 만든다.
        on_delete=models.SET_NULL,
        # related_name은 강사한테 연결되어 있는 사람들은 students라고 하면 좋은데,
        # user_set이라고 해야하니까, related_name을 변경해주는 것
        related_name='students',
        blank=True,
        # 실제 데이터베이스에 적용이 되는 부분, 데이터베이스에서 이 필드가 null을 허용한다.
        null=True
    )
```
추가

이 외에도, on_delete에는(CASCADE, SET_NULL)
- protect: 지우는 걸 막는다.
- set_default: 실제로 id값에 해당하는 데이터가 존재하는 지를 확인해야만 사용이 가능하다.
- set: ()에 함수를 지정해서, 함수에 따라 다르게 들어가도록 지정한다. set_default와 다르게,
- do_nothing: 아무것도 안하는것으로 데이터베이스에서 비활성 에러가 뜬다. foreignkey를 통해 관계가 정의가 되어있으면 항상 그 관계는 옳아야 하는데, 이렇게 데이터베이스 차원에서 오류가 발생하면 그건 장고에서 해결이 불가능하다.

#### ManyToManyField

many-to-many관계를 정의하기 위해서는 `ManyToManyField`를 사용한다.

`ManyToManyField`는 관계를 정의할 모델 클래스를 인수로 가져야 한다.

python manage.py startapp many_to_many 후 만들어진 앱을 models패키지 안에 넣는다.
apps.py를 수정하고 setting.py의 INSTALLED_APPS에 추가한다. 이후 models.py작성한 다음 `makemigrations`, `migrate`

```python

class Topping(models.Model):
    name = models.CharField(max_length=50)

    def __str__(self):
        return self.name


class Pizza(models.Model):
    name = models.CharField(max_length=50)
    toppings = models.ManyToManyField(
        Topping,
        related_name='pizzas',
    )

    def __str__(self):
        return self.name
```

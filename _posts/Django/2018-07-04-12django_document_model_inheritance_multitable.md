---
layout: post
title: Django - django-document 6, introduction to models (Model Inheritance / multi table)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django-document을 보고 정리했습니다.

<hr>

### Multi-table inheritance

상속을 하나 거칠때마다 테이블이 하나씩 더 생긴다. 여러개의 테이블을 사용해서 상속을 구현한 것으로 계층 구조의 각 모델이 모두 각각 자신을 나타내는 모델일 때를 말한다. 이는 즉 부모클래스, 자식클래스도 모두 데이터베이스 테이블로 나타난다는 것을 의미한다.

이 상속관계는 자동으로 생성 된 OneToOneField를 통해 자식 모델과 각 부모 간의 링크를 만든다.

```python
from django.db import models

class Place(models.Model):
    name = models.CharField(max_lengnth=50)
    address = modls.CharField(max_lengnth=80)

class Restaurant(Place):
  serves_hot_dog = models.BooleanField(default=False)
  serves_pizza = models.BooleanField(default=False)
```

지금 Restaurant은 Place를 abstract=True없이 그냥 상속을 받오 있는데, 이 경우에는 데이터베이스 테이블에 Place와 Restaurant 둘 다 존재한다.

```shell
from inheritance.multi-table.models import Place, Restaurant
r1 = Restaurant.objects.create(name='패스트캠퍼스', address='성수동 제강빌딩')
r1.name
>>> '패스트캠퍼스'
r1.address
>>> '성수동 제강빌딩'

r1.serves_hot_dog
>>> False

r1.serves_pizza
>>> False

p1 = Place.objects.create(name='제주국수', address='성수동')
```
데이터를 생성한 후 테이블을 보면 `multi_table_place`에는 id 필드와 name, address필드가 생성되어있고 multi_table_restaurant에는 place_ptr_id와 serves_pizza, serves_hot_dog 필드가 있다.

multi_table_restaurant에는 id 필드가 없고 place_ptr_id필드가 있는데, 이 필드가 multi_table_place의 id필드와 연결되는 자동으로 생긴 OneToOneField이다.  

그리고 OneToOneField에 따로 parent_link=True옵션을 주지 않았는데, 이는 자동으로 생기고 그게 바로 ptr_id를 말한다. 이는 포인터의 개념으로 보면 되는데, 해당 id값이 어디를 가리키고 있는지를 말하고 있다.

이떄 이 값을 우리가 임의로 지정할 수 있는데, 그때 사용하는게 parent_link=True이고 그렇지 않으면 이 값은 자동으로 생성된다.

#### Meta and multi-table inheritance

다중 테이블 상속 상황에서 자식 클래스가 부모의 Meta클래스에서 상속받는 것은 의미가 없다. 모든 메타 옵션은 이미 상위 클래스에 적용되었고 다시 적용하면 모순된 행동만 발생한다. 이는 이 상속받은 클래스는 기본키를 부모꺼를 따라가고, 이러한 상황에서 Place, Restaurant둘다 이름을 오름차순 정렬을 하고싶다고 가정해보자.

```python
from django.db import models

class Place(models.Model):
    name = models.CharField(max_lengnth=50)
    address = modls.CharField(max_lengnth=80)

    class Meta:
      ordering = ['name']

class Restaurant(Place):
  serves_hot_dog = models.BooleanField(default=False)
  serves_pizza = models.BooleanField(default=False)
```
이때 Restaurant이 ordering을 받아서 오름차순을 하는게 의미가 있는가를 따져보면 없기 때문에, 상속 받는게 의미가 없다. 만약에 Meta를 상속받을때, Place를 상속 그대로 받으면 사실 Meta를 사용하지 않아도 되는거니까

따라서 자식 모델은 부모의 메타 클래스에 엑세스 할 수 없지만, 그래도 자식이 부모로부터 동작을 상속하는 몇 가지 제한된 경우는 있다. `ordering`, `get_latest_by`

무보가 ordering이 있고 이를 자식이 해제하고싶을때 명시적으로 사용을 중지할 수 있다.

```python
class ChildModel(ParentModel):
  #...
  class Meta:
    ordering = []
```

#### Inheritance and reverse relations

다중 테이블 상속은 암시적인 OneToOneField를 사용해서 부모와 자식을 연결하기 때문에 위의 예와 같이 상위에서 하위로 이동할 수 있다.(`ptr_id`) 그러나 이 경우 related_name의 값으로 ForeignKey 및 ManyToManyField관계에 대한 기본 값을 사용한다.

```python
class Supplier(Place):
  customers = models.ManyToManyField(Place)
```

결과는 다음과 같은 에러를 나타낸다.

```python
Reverse query name for 'Supplier.customers' clashes with reverse query
name for 'Supplier.place_ptr'.

HINT: Add or change a related_name argument to the definition for
'Supplier.customers' or 'Supplier.place_ptr'.
```
customers와 place_ptr이 충돌한다는 것으로 사실

```python
lass Supplier(Place):
  # place_ptr = models.OneToOneField(Place)가 생략되어 있다.
  customer = models.ForeignKey(Place, on_delete=models.CASCADE)
```
이렇게 되어있을때, 특정 Place인 p1이 있을 때
Supplier.objects.filter(customer=p1)와 Supplier.objects.filter(place_ptr=p1)인 경우는 다를 것이다.

즉, Supplier의 목록중에 customer가 p1인 경우의 Supplier와 Supplier.objects.filter에서 place_ptr에 해당하는 게 p1인 것은 다르다. 근데 문제는 방금은 정방향이지만 역방향일떄가 문제이다.

p1이 역방향 Manager를 사용할 때, 즉 ForeignKey로 연결되어있을때, Place 본인이 customer인 Supplier을 찾고 싶은 상황이다. 그러면 이건 OneToOneField이 아닌 ForeignKey이니까 `p1.supplier_set`

근데 만약, OneToOneField일 경우에는 `p1.supplier`만 하면된다. OneToOneField에는 set을 사용하지 않는다. 따라서 `related_name`이 겹치지 않는다.

그런데 p1이 역방향 Query를 사용할 때, 오류가 발생한다.

related_name과 related_query_name에 아무것도 지정하지 않을 때의 기본값은 클래스명의 lowercasedlsep, 그렇기 때문에 현재의 related_query_name은 `supplier`로 겹치고 있다. (related_name은 supplier_set과 supplier로 안겹친다.)

따라서 related_name을 변경해주자

```python
lass Supplier(Place):
  # place_ptr = models.OneToOneField(Place)가 생략되어 있다.
  customer = models.ForeignKey(
    Place,
    on_delete=models.CASCADE,
    related_name='supplier_by_customer',
  )
```

related_name은 안겹쳤지만, 얘를 지정해주고 나면 related_query_name도 자동으로 따라가기 때문에, related_name을 변경해준것

> p1.supplier_set <-MTO
> p1.supplier <-OTO (역방향 매니저가 존재하지 않음)

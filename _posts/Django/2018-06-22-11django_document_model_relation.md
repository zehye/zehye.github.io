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
    # related_name: manufacturer의 입장에서 자기와 연결된 car를 참조할 떄 어떤 이름을 쓸 것 인가?

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
        # 여기서도 특정 Topping을 가지고 있는 피자목록을 가져오기 위해서는
        # 피자치즈.pizza_set을 해야하는데, 이를 간편하게 하기 위해 related_name을 지정해주는 것이다.
        related_name='pizzas',
    )

    def __str__(self):
        return self.name
```

**Extra fileds on many_to_many relationships**

피자와 토핑같은 간단한 many_to_many 관계를 만들때, `ManyToManyField`는 필요로 하는 모든것을 제공한다. 하지만, 떄때로 두 모델 사이의 관계와 데이터를 연결해야 할 수도 있다.

예를 들어, 음악가가 속한 그룹을 트래킹(추적)하는 경우, 사람과 그룹으로 멤버를 이루는 관계에서 `ManyToManyField`를 통해 관계를 나타내려고 한다. 그러나 어떤 사람이 그룹으로 가입하는 날짜와 같은 세부사항들이 추가로 존재한다고 했을 때, 장고에서는  many_to_many관계를 사용되는 모델로 지정할 수 있다.

그리고 중간모델에 추가 필드를 넣을 수 있고 `ManyToManyField`의 `through`인수에 중간 모델을 가리키도록 하여 연결할 수 있다.

```python
from django.db import models

class Person(models.Model):
    name = models.CharField(max_length=128)

    def __str__(self):
        return self.name


class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(
        Person,
        # through를 통해서 특정 클래스를 적어줬고, 문자열로 적어준 이유는 아래에 해당 클래스가 정의되었기 때문
        # memberships이라는 클래스가 새로운 테이블이 되면서 이 테이블이
        # Person과 Group의 many_to_many관계를 나타내기 위해서 쓰인다.
        through='Membership',
    )

    def __str__(self):
        return self.name


class Membership(models.Model):

  # person과 group은 foreignkey로 연결이 되어있기 때문에 db table에는 _id로 저장되어있다.
    person = models.ForeignKey(
        Person,
        related_name='memberships',
        on_delete=models.CASCADE,
    )
    group = models.ForeignKey(Group, on_delete=models.CASCADE)
    recommender = models.ForeignKey(
        Person,
        related_name='membership_by_recommender',
        on_delete=models.SET_NULL,
        blank=True,
        null=True)

    # 아래 두 필드는 그냥 추가가 된다.
    date_joined = models.DateTimeField()
    invited_reason = models.CharField(max_length=64)

    def __str__(self):
        return '{person} - {group} ({date})'.format(
            person=self.person.name,
            group=self.group.name,
            date=self.date_joined,
        )
```

이렇듯 중간모델 (class Membership)을 설정할 때, 명시적으로 `many_to_many`관계에 참여하는 모델들의 `foreignkey`를 지정한다. 이 명시적인 선언은 두 모델이 관련되는 법을 정의한다.

중간모델에는 몇 가지 제한 사항이 있다.

* 중간 모델은 원본모델(ManyToManyField가 지정된 Group 모델)에 대해 단 하나의 foreignkey를 가져야 한다. 여러개여도 안되며, 없어도 안된다. 아니면 반드시 `ManyToManyField`에서 `through_fields`옵션으로 관계에 사용될 필드 이름을 지정해줘야 한다. 둘 중 하나가 아니면 `Validation`에러가 발생한다. 타겟모델(Person모델)의 경우에도 동일하다.

```python
many_to_many.Group.members: (fields.E335) The model is used as an intermediate madel by 'many_to_many.Group.members', but it has more than one foreign key to 'Person', which is ambigous. You musy specify which foreign key Django should use via the through_fields keyword argument.
```

이 말은
```python
class Membership(models.Model):

    person = models.ForeignKey(
        Person,
        on_delete=models.CASCADE,
    )
    group = models.ForeignKey(Group, on_delete=models.CASCADE)
    recommender = models.ForeignKey(
        Person,
        on_delete=models.SET_NULL,
        blank=True,
        null=True)
```
이렇게 person과 group이 ForeignKey를 통해서 Person, Group모델을 받고 있는데, recommender가 ForeignKey로 또 person을 받고 있으면 안된다는 것으로 이때 이렇게 사용하기 위해서는 `through_fields`를 이용해 사용될 필드 이름을 정의해줘야 한다.

```python
class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(
        Person,
        through='Membership',
        through_fields = ('person', 'group')
    )

    def __str__(self):
        return self.name
```
근데 이렇게 적어도 오류가 뜰것이다.

`through_fields`의 방향 또한 설정해줘야 하기 때문이다.

```python
class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(
        Person,
        through='Membership',
        through_fields = ('group', 'person')
    )

    def __str__(self):
        return self.name
```
through_fields는 ('field1', 'field2')를 받으면서 field1은 ManyToManyField가 정의된 모델의 foreignkey이름이고 field2는 foreignkey의 이름을 받는다.

이 경우 field1인 소스모델은 group이고 field2인 타겟모델은 person이기 때문에 그 순서에 맞게 작성해줘야 해당 에러는 뜨지 않는데, 아직 에러가 하나 더 남아있다.

```python
many_to_many.Membership.person: (fields.E304) Reverse accessor for 'Membership.person' clashes with reverse accessor for 'Membership.recommender'.
many_to_many.Membership.recommender: (fields.E304) Reverse accessor for 'Membership.recommender' clashes with reverse accessor for 'Membership.person'.
```

이는 Membership모델에서 우리가 p1이라는 Person인스턴스가 있을 때,

Membership.objects.filter(person=p1)과 Membership.objects.filter(recommender=p1)이라고 하면 p1.memberships_set.all()이면 p1이 person의 입장인지 recommender의 입장인지를 알 수가 없게된다.

그래서 이럴 때 이름을 변경해줘야 한다. (related_name이 겹치게 된다는 뜻)

따라서 related_name을 따로 지정해줘야 한다.

```python
from django.db import models

class Person(models.Model):
    name = models.CharField(max_length=128)

    def __str__(self):
        return self.name


class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(
        Person,
        through='Membership',
        through_fields=('group', 'person'),
    )

    def __str__(self):
        return self.name


class Membership(models.Model):

    person = models.ForeignKey(
        Person,
        related_name='memberships',
        on_delete=models.CASCADE,
    )
    group = models.ForeignKey(Group, on_delete=models.CASCADE)

    recommender = models.ForeignKey(
        Person,
        related_name='membership_by_recommender',
        on_delete=models.SET_NULL,
        blank=True,
        null=True)

    date_joined = models.DateTimeField()
    invited_reason = models.CharField(max_length=64)

    def __str__(self):
        return '{person} - {group} ({date})'.format(
            person=self.person.name,
            group=self.group.name,
            date=self.date_joined,
        )
```

이제 `ManyToManyField`에서 중간모델(`Membership`)을 사용할 수 있으므로 중간모델 인스턴스를 만들어 본다.

```python
>>> ringo = Person.objects.create(name="Ringo Starr")
>>> paul = Person.objects.create(name="Paul McCartney")
>>> beatles = Group.objects.create(name="The Beatles")
>>> m1 = Membership(person=ringo, group=beatles,
...     date_joined=date(1962, 8, 16),
...     invite_reason="Needed a new drummer.")
>>> m1.save()
>>> beatles.members.all()
<QuerySet [<Person: Ringo Starr>]>
>>> ringo.group_set.all()
<QuerySet [<Group: The Beatles>]>
>>> m2 = Membership.objects.create(person=paul, group=beatles,
...     date_joined=date(1960, 8, 1),
...     invite_reason="Wanted to form a band.")
>>> beatles.members.all()
<QuerySet [<Person: Ringo Starr>, <Person: Paul McCartney>]>
```

일반적인 many_to_many필드와는 달리 우리가 명시적으로 중개테이블을 만들었을 경우, add(), create(), set()명령어를 사용할 수 없다.

```python
beatles.members.add(paul)
>>> Cannot use add() on a ManyToManyField which specifies an intermediate model. Use many_to_many.Membership's Manager instead.
```

우리가 명시적으로 만들고 추가 필드를 만들었기 때문에, add를 하는게 아니라 membership테이블을 만들어줘야 한다.  

```python
Membership.objects.create(
  person=paul,
  group=beatles,
  date_joined=date(1960, 8, 1),
  invite_reason="Wanted to from a band"
)

# members의 name이 paul로 시작하는 멤버가 있는 경우의 Group
Group.objects.filter(members__name__startswith='paul')
>>> <QuerySet [<Group: The Beatles>]>


Group.objects.filter(members__name__contains='a')
# 결과값이 두개가 나오는 이유는 앞서 paul과 ringo star 모두 이름에 a를 갖기 떄문이다.
>>> <QuerySet [<Group: The Beatles>, <Group: The Beatles>]>

# 이 중복을 없애기 위해서
Group.objects.filter(members__name__contains='a').distinct()
>>> <QuerySet [<Group: The Beatles>]>
```

이때 너무 많아지는 model을 구분하기 위해 models.py를 package로 만들어보자.

models.py를 우클릭하면 `Convert to Python Package`를 누른다. 그러고나면 models의 `__init__.py`에 우리가 적었던 model들이 다 들어가있다.

> 이때 `__init__.py`의 파일에 들어간 자료를 `basic.py`와 `intermediate.py`에 나눠 적어놓고 migration을 하면 모든 db가 날아갔다는 메시지를 받을 것이다. migrate하면 망함

그래서 `__init__.py`에서 패키지 자체를 로드 했을때 아래 모듈에 있는 클래스들을 다 가져올 수 있어야 한다.

```python
from .basic import *
from .intermediate import *
```

이렇게 만들때는 각 모듈에서

```python
# 튜플이기 때문에 ,는 필수
# __init__.py에서 모듈을 불러올때 아래처럼 지정을 해놓으면 __all__에 지정한 애들만 불러올 수 있다.
__all__ = (
  'Topping',
  'Pizza',
)

__all__ = (
  'Person',
  'Group',
  'Membership',
)
```
여기까지 했으면 앞서 잘못 만들었던 migration은 없애줘야한다.

아직 git에 넣은 상황이 아니기 때문에 그냥 delete해주면 된다.

**many_to_many에서 self를 쓰는 방법**

쓸일은 언제일까?

```python
from django.db import models

__all__ = (
  'FacebookUser',
)

class FacebookUser(models.Model):
  name = models.CharField(max_length=50)
  friends = models.ManyToManyField(
  # 내가 친구추가를 하면 상대방도 나를 참조할 수 있도록 하는 것

  # 관계가 대칭적으로 형성된다.
  # A가 B를 friends에 추가 -> B의 friends에도 A가 추가되어있음
    'self',
  )
  def __str__(self):
    return self.name
```

shell에서 실행해보면
```python
from models.many_to_many.models import FacebookUser

u1 = FacebookUser.objects.create(name='홍길동')
u2 = FacebookUser.objects.create(name='고길동')
u3 = FacebookUser.objects.create(name='박보검')
u4 = FacebookUser.objects.create(name='강동원')

u1.friends.add(u2, u3)
u1.friends.all()
>>> <QuerySet [<FacebookUser: 고길동>, <FacebookUser: 박보검>]>

u2.friends.all()
# 페이스북에서는 내가 친구를 걸어서 상대방이 오케이만 하면 서로 친구관계가 되니까
# 이를 대칭적이라고 하는데, 한쪽에서 반대쪽으로 관계를 만들면 반대쪽에서 원본쪽으로도 관계를 만든다.

# 이는 many_to_many를 했을때 자기자신을 받으면 나오는 기본관계
>>> <QuerySet [<FacebookUser: 홍길동>]>
```

이때 새로운 함수를 만들어보자

```python
from django.db import models

__all__ = (
  'FacebookUser',
)

class FacebookUser(models.Model):
  name = models.CharField(max_length=50)
  friends = models.ManyToManyField(
    'self',
  )

  def __str__(self):
    return self.name

  def show_friends(self):
    print('{}의 친구목록'.format(self.name))
    for friend in self.friends.all():
      print('-{}'.format(friend.name))
    print('(총{}명)'.format(len(self.friend.all())))
```

이제 친구 추가 시 대칭적인 관계가 아닌 것(`symmetrical=False`)에 대해 알아보겠다.

자기와의 다대다 관계에서 대칭을 원하지 않을 경우 `symmetrical=False`를 설정하고 이렇게 하면 Django가 역방향 관계에 대한 설명자를 추가하여 `ManyToMany`가 비대칭이 될 수 있도록 해준다.

```python
from django.db import models

__all__ = (
  'InstagramUser',
)

class InstagramUser(models.Model):
  name = models.CharField(max_length=50)

  # 내가 follow하고 있는 사람들의 목록
  following = models.ManyToManyField(
    'self',
    symmetrical = False,
  )

  def __str__(self):
    return self.name
```
이를 shell에서

```python
u1 = InstagramUser.objects.create('홍길동')
u2 = InstagramUser.objects.create('고길동')
u3 = InstagramUser.objects.create('박보검')
u4 = InstagramUser.objects.create('강동원')

u1.save()
u2.save()
u3.save()
u4.save()

# 혹은 튜플 언패킹으로 풀 수도 있다.
u1, u2, u3, u4 = InstagramUser.objects.all()

# 이때 u2가 u1을 팔로잉
u2.following.add(u1)

u2.following.all()
>>> <QuerySet [<InstagramUser: 홍길동>]>
u1.following.all()
>>> <QuerySet []>

u3.following.add(u1)

# 이때 u1에서 나를 팔로우하고 있는 사람의 목록을 보고싶을 땐, set
# 그러나 이는 맞는 표현이 아니다. related_name을 추가
u1.InstagramUser_set.all()
```

```python
class InstagramUser(models.Model):
  name = models.CharField(max_length=50)
  following = models.ManyToManyField(
    'self',
    symmetrical = False,
    related_name='followers'
  )
```

더 나아가 self, symmetrical=False, intermediate를 하는 방법을 알아보자.

```python
from django.db import models
__all__ = (
  'TwitterUser',
  'Relation',
)
class TwitterUser(models.Model):
  """
  User간의 관계는 2종류로 나뉜다, follow / block
  """
  # TwitterUser에 대해서 어떤 유저가 다른 유저와 어떤 관계를 만들어내고 있는지,
  # 관계를 하나 만들어내고 있고, 그 관계가 여러개가 되면 실제 sns에서 쓰는 관계가 만들어진다.
  name = models.CharField(max_length=50)
  reltations = Models.ManyToManyField(
    'self',
    symmetrical=False,
    through='Relation'
  )

  def __str__(self):
    return self.name

class Relation(models.Model):
  """
  TwitterUser간의 MTM관계를 정의한다.
    from_user, to_user, follow인지 block인지
  """

  CHIOCES_RELATION_TYPE=(
    ('f', 'follow'),
    ('b', 'block'),
  )

  from_user = models.ForeignKey(
    Twitteruser,
    on_delete=models.CASCADE,
    related_name='relations_by_from_user',
  )
  to_user = models.ForeignKey(
    TwitterUser,
    on_delete=models.CASCADE,
    related_name='relations_by_to_user',
  )
  # 입력값을 제한하는 choices옵션 추가
  relation_type= models.CharField(
    max_length=1,
    choices=CHIOCES_RELATION_TYPE,
  )
  # 관계의 생성일을 추가
  # 새로운 record가 생기는 순간 저장, 나중에 수정을 한다고 해도 저장이 안됨
  created_at = models.DateTimeField(auto_now_add=True)

  def __str__(self):
    # get_FOO_display()함수를 사용해서 choices를 사용한 필드의 출력값을 사용
    return 'from({}), to({}), {}'.format(
      self.from_user.name,
      self.to_user.name,
      self.get_relation_type_display(),
    )
```
> 우리가 Relation에 대해 from_user, to_user를 알아볼때에는 겹치는게 없으니까 상관없는데 TwitterUser와 관계되는 Relation을 알고 싶을때는 relations_set.all()을 하게되는데, 이때 from_user와 to_user가 겹치게된다. 이런 상황을 대비하여 우리가 지정하는 것이 related_name이다.

shell에서 유저와 관계를 설정해보자.
```python
u1, u2, u3, u4 = [TwitterUser.objects.create(name=name) for name in ('홍길동', '고길동', '박보검', '강동원')]

# 이때 User 4명을 만들었는데, 이들 사이에서 새로운 관계를 만들때, 지금 중개모델을 사용하고 있으니까 add를 사용하는 것은 안된다.

Relation.objects.create(
  from_user=u1,
  to_user=u2,
  relation_type='f',
)
Relation.objects.create(
  from_user=u1,
  to_user=u3,
  relation_type='f',
)

Relation.objects.create(
  from_user=u1,
  to_user=u4,
  relation_type='b',
)

Relation.objects.create(
  from_user=u2,
  to_user=u4,
  relation_type='f',
)

Relation.objects.all()
>>> 모든 관계에 대한 objects가 나온다.

# from이 홍길동이고, relation_type = 'f'인 QuerySet
Relation.objects.filter(from_user='홍길동', relation_type='f')
```


#### OneToOneField

one-to-one관계를 정의하려면, `OneToOneField`를 이용하면 된다. 다른 관계 필드와 마찬가지로 모델 클래스의 속성을 선언하면 된다.

일대일 관계는 다른 모델을 확장하여 새로운 모델을 만드는 경우 유용하게 사용가능하다.

이는, 기존 모델은 그대로 두고 모델에 추가 정보가 들어갈때 - User정보가 있는데, 이는 굉장히 많이 불러오는 데이터로 추가 정보가 많을때, 그 정보를 무조건 한 테이블에 가지게 되면 Query를 날릴때, 항상 신경을 써야한다.

그러기에는 힘드니까, 추가 정보는 테이블 검색할때 아예 검색이 안되게 다른 테이블에 구성을 해놓고 해당 테이블에 연결만 해놓으면 된다. 그러므로 해당 추가 정보는 일대일로 연결을 하면 된다.

예로들어, 장소(Places)정보가 담긴 데이터베이스를 구축한다고 할 때, 이미 데이터베이스에는 주소, 전화번호 등의 정보가 들어가 있을 것이다. 그런데, 맛집 데이터베이스를 추가적으로 구축할 경우, 새로 Restaurant모델을 만들수도 있지만, 반복을 피하기 위해 Restaurant모델에 Place모델만 `OneToOneField`로 선언해 줄 수 있다.

```python
from django.db import models

class Place(models.Model):
  name = models.CharField(max_length=50)
  address = models.CharField(max_length=80)

  def __str__(self):
    return '{}s the place'.format(self.name)

class Restaurant(models.Model):
  place = models.OneToOneField(
    Place,
    on_delete=models.CASCADE,
    # 레스토랑의 id가 딱히 필요없어서 사용
    # primary_key=True인데, OneToOneField이다 = 이 레스토랑의 primary_key는
    # Place의 값을 그대로 따라간다. Place와의 연결을 지어주는 Field역할을 하면서,
    # 기본키 또한 Place를 따라가게 된다.

    # 즉, 레스토랑을 특정지을 수 있는 key가 Place의 키와 같은 값을 공유하고 있다.
    primary_key=True,
  )
  serves_hot_dogs = models.BooleanField(default=False)
  serves_pizza = models.BooleanField(default=False)

  def __str__(self):
    return '{}s the restaurant'.format(self.place.name)
```

기본적으로 Restaurant은 Place가 있어야 존재 하지만 Place는 Restaurant이 없어도 존재할 수 있다. 그래서 Restaurant을 기준으로 place를 찾는 경우 오류가 나는 경우는 거의 없지만 그 반대의 경우 오류가 날 수도 있다.

```python
from django.core.exception import ObjectDoesNotExists

try:
  p1.restaurant
except ObjectDoesNotExists:
  print('There is no restaurant here')

# 따라서 이를 미리 알아보기 위해
hasattr(p1, 'restaurant')
# -> p1에 restaurant속성을 가지고 있냐는 파이썬 내장함수
```

---
layout: post
title: Django - django-document 1, introduction to models (relation 전까지)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django-document을 보고 정리했습니다.

<hr>

## models

모델은 데이터에 대한 정보를 나타내는 최종소수이다. 데이터의 필수 필드와 행동(함수) 포함한다.

일반적으로, 각각의 모델은 데이터베이스 테이블에 매핑된다.

### Quick example

이 샘플 모델은 `first_name`과 `last_name`을 가진 `Person`을 정의한다.

```python
from django.db import models

class Person(model.Model):
  frist_name = models.CharField(max_length=30)
  last_name = models.CharField(max_length=30)
  # 여기서의 first_name 과 last_name이 모델의 field를 의미한다.
  # 각각의 필드는 클래스의 속성을 나타내며, 데이터베이스의 컬럼(열)에 매핑된다.
```

이 하나의 클래스 모델이 데이터 베이스 테이블에 매핑되는 것을 의미한다.

데이터 베이스 테이블에 매핑된다는 것은 우리가 `db browser for sqlite`에서 본 테이블을 의미한다.

몇 가지 기술적 정보:

- 테이블의 이름은 `myapp_person`으로 만들어진다. 이는 재정의 할 수 있다.

  - 이때 `myapp`은 `app`의 이름을 뜻하고 `person`은 `Person`클래스의 소문자화 된 것으로 우리가 기본적으로 `db browser for sqlite`에서 `_소문자 클래스 명`으로 봤던 것을 떠올릴 수 있다.

- `id`필드는 자동으로 추가되지만, 오버라이드 할 수 있다.

### Using models

모델을 정의하면, 장고에게 이 모델을 사용할 것임을 알려줘야 한다. 설정 파일에서 `INSTALLED_APPS`설정에 `models.py`를 포함하고 있는 모듈의 이름을 추가해준다.

```python
INSTALLED_APPS = [
  'myapp',
  # 혹은
  'myapp.apps.MyappConfig',
]
```

새로운 어플리케이션을 `INSTALLED_APPS`에 추가했다면, `manage.py migrate`명령어를 실행해준다. 우리가 애플리케이션을 직접 만들게 되면 데이터베이스의 변경사항을 가지고 있지않은데, 남이 만들어 놓은 경우에는 그 사람이 그 애플리케이션이 동작하기 위해 만들어놓은 데이터베이스 변경사항이 있다.

예로 들어, 기본적으로 추가되어 있는 AUTH application(인증정보 관련) 이는 회원정보를 관리하기 위한 테이블정보를 가지고 있는데, 이런 애들을 우리가 설정해놓은 데이터베이스에 실제로 테이블로 만들어 주는 것을 의미한다.

`manage.py makemigrations`을 통해서 우리가 클래스를 생성하거나, 클래스의 필드를 변경하거나, 클래스 필드의 속성을 변경할 때 그러한 변경사항들을 데이터베이스에 반영하기 위한 변경사항에 대한 데이터베이스 파일을 만들어준다.


## Fields

모델에서 가장 중요한 부분이며, 반드시 필요한 부분이다. 필드는 데이터베이스의 필드를 정의한다. 또한 필드는 클래스 속성으로 사용된다.

clean, delete, save와 같은 models API와 중복되지 않도록 한다.

```python
from django.db import models

class Musician(models.Model):
  # CharField는 문자를 저장하기 좋은 필드
  first_name = models.CharField(max_length=50)
  last_name = models.CharField(max_length=50)
  instrument = models.CharField(max_length=100)

class Album(models.Model):
  artist = models.ForeignKey(Musician, on_delete=models.CASCADE)
  name = models.CharField(max_length=100)
  # DateTimeField는 날짜를 저장하기 좋은 필드
  release_date = models.DateTimeField()
  # IntegerField는 정수형 데이터를 저장하기 좋은 필드
  num_stars = models.IntegerField()
```

### Field types

각각의 필드는 적절한 필드 클래스의 인스턴스여야 한다.

```python
class Musician(models.Model):
  # models라는 모듈안에 있는 CharField라는 클래스를 호출했으니까
  # 생성자가 실행되면서 인스턴스가 first_name에서 할당되는 것
  first_name = models.CharField(max_length=50)
```

- 데이터베이스 컬럼의 데이터 형 (CharField, DateTimeField, IntegerField 등)

- form field를 렌더링할 때 사용할 기본 HTML위젯

  - 이전에는 form을 만들때 글을 쓰는 form을 만들었다(html 파일)

  - 현재 클래스가 받고 있는 form은 총 세개(이름, 성, 악기)

  - 기존에는 이 세개에 대한 form을 만들어서 input을 받을 수 있었는데, 우리가 클래스를 통해 만들어 놓은 그 form을 현재 클래스와 바로 연동을 해서 어떤 form을 쓰고싶어라고 하면 그 form을 장고가 알아서 만들어준다.

- Django admin에서 자동으로 만들어지는 form의 검증 형태


### Field options

각 필드는 고유의 인수를 가진다. 예로들어 `CharField`는 `max_length`인수를 반드시 가져야 한다.

아래는 각 필드에 인수들로 들어올 수 있는 인수들에 대해 설명한다.

#### null

기본값은 False이며, True일때 장고는 빈 값을 NULL로 데이터베이스에 저장한다.

데이터베이스의 값이 없어도 된다는 것을 의미한다. 빈 값이 아니다 값이 없다는 점이 중요하다.

#### blank

기본값은 False이며, True일 경우 필드는 빈 값을 허용한다.

**null** 과 **blank** 는 의미가 전혀 다르다. null은 비어있는 것이고(아예 빈 값이 허용이 되는 것) blank은 빈 값이다(빈 값이 들어가 있는 것/ 데이터 베이스에 빈 문자열 값("" "")을 허용하는 것을 뜻한다).

즉 blank는 없어도 뭐라도 들어있어야 하는 것이고(다 쓴 휴지심) null은 아예 비어있는 것을 의미한다. (비어있는 화장지)

#### choices

반복가능한 튜플의 묶음을 선택목록으로 사용한다. 이 인수가 주어지면, 기본 폼 위젯은 select box로 대체되어 선택값을 제한한다.

```python
SHIRT_SIZES = (
# 기본적으로 choices는 CharField를 쓰는데,
# (데이터 베이스에 저장되는 것, 표시를 할 때 쓰는 것)
# 문자열을 저장할때, 단순 분류를 위해 표시를 하려고 할때, 긴 문장이나 한글이 들어가는 걸 피하기 위해 사용된다.
# 그 대신에 사용할 때(표시될 때는), 풀어쓴 문자를 사용하고 싶은 것이다. (효율의 차원)
    ('S', 'Small'),
    ('M', 'Medium'),
    ('L', 'Large'),
# 클래스 자체의 속성일때, 모듈 차원의 변수일때 쓰는 변수는 대문자 사용이 가능하다.
)
shirt_size = models.CharField("셔츠 사이즈", help_text="S는 작음", max_length=1, choices=SHIRT_SIZES)
```

document/ app/ `models 패키지`를 만들어준다.

그리고 cd models 안에서 python ../manage.py startapp fields(상위폴더)를 하면서 models 패키지 안에 `fields 앱`을 만들어준다.

애플리케이션은 어느 위치에 있어도 상관이 없는데 우리가 하는 startapp이 만들어주는 패키지는 특별한 게 아니라 애플리케이션이 가져야할 기본 구조를 만들어주는 것으로 어느 구조 안에 애플리케이션이 존재해도 상관이 없다.

대신 우리가 만든 방법을 하게 되면 field/ apps.py를 조금 바꿔주어야 한다.

```python
from django.apps import AppConfig

class FieldsConfig(AppConfig):
  # models 패키지 안에 있는 fields 패키지 이름을 나타내는 것
  # 이 application의 파이썬 full path
  name = 'models.fields'
```

예전에 비해서 애플리케이션에서 가지고 있어야할 정보의 양이 많아지게 되면서, 많아진 애플리케이션이 가지고 있는 메타데이터를 나타내는 방법이 없어지게 되면서, `AppConfig`라는 게 추가되었다.

현재 우리는 models라는 패키지 안에서 많은 애플리케이션들을 만들건데, 이게 패키지 바깥에 있으면 보기가 힘들어지니까 안쪽으로 집어넣는 과정을 한번 만들어준 것이다.

models.py에서

```python
from django.db import models


class Person(models.Model):
    SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )
    name = models.CharField(max_length=60)
    # "셔츠사이즈"는 Verbose name
    shirt_size = models.CharField("셔츠 사이즈", help_text="S는 작음", max_length=1, choices=SHIRT_SIZES)
```
ipython shell에서 보면

```python
# models패키지의 fields 앱, models class
from models.fields.models import Person

p = Person(name='pjh', shirt_size = 'S')
p.save()

p.shirt_size
>>> 'S'

# get_FOO_display()에서 FOO는 필드 이름
# 파이썬이 동적으로 FOO에 들어가는 필드값을 판단해서 그 값에 해당하는 필드가 가진 choices의 값을 알아서 가져온다.
p.get_shirt_size_display()
>>> 'Small'
```

그러고 이 모델이 데이터베이스에 저장될 수 있도록 INSTALLED_APPS에 추가해준다.

```python
INSTALLED_APPS = [
  'models.fields.apps.FieldsConfig',
]
```

그렇게 하고 난 뒤 `python manage.py makemigrations` `python manage.py migrate`해준다.

#### default

주어지지 않았을 때, 필드에 기본값으로 설정된다.

#### help_text

가능하면 적어주는 게 좋은데, 이 필드가 어떤일을 하는 것인지 등 필드가 많을 때 그 필드가 정확히 어떤 기능을 하는지 보여주는 것이 좋다.

#### primary key

primary key가 True일 경우, 해당 필드는 primary key로 사용된다. (id값이 생기지 않는다.)

primary key필드는 읽기전용으로 기존 개체의 primary key값을 변경한 후 저장하면 이전 객체와는 별개의 새로운 객체가 생성된다.

ipython shell에서 보면

```python
p = Person(name='pjh', shirt_size = 'S')
p.save()

p.shirt_size
>>> 'S'

p.id
>>> 1

p.shirt_size = 'L'
Person.objects.get(id=1)
>>> 'S'
# 아직 p.save()를 하지 않았으니까

p.save()
Person.objects.get(id=1)
>>> 'L'

p.id = 100
p.shirt_size = 'S'
p.save()

Person.objects.get(id=100)
>>> 'S'
# 그러나 테이블은 id가 1인경우와 id가 100인 경우 두개가 생겼다.
# id값을 바꾸면 id값은 변경이 안된다.
# 읽기 전용으로 덮어씌우는 것이 불가능하다.
```

#### unique

unique가 True 일 경우, 이 필드의 값은 테이블 전체에서 고유해야한다.

이는 보통 중복을 피하고 싶을 때, (회원가입 시 id, username, nickname 등)


### Automatic primary key fields

primary key는 하나의 필드만을 가져야한다. 하나의 필드만이 primary key를 고유하게 나타낼 수 있다.

### Verbose field names

`ForeignKey`, `ManyToManyField`, `OneToOneField`를 제외한 모든 필드에서 자세한 필드명은 첫번째 인수이다.

위 세개의 필드는 다른 객체와의 연결을 나타내는 필드인데, 이들을 제외하고 모든 필드에서 자세한 필드명 즉 Verbose name이 항상 첫번째 인수이다.

만약 Verbose name이 주어지지 않을 경우, 장고는 자동으로 해당 필드의 이름을 사용해서 Verbose name을 만들어서 사용한다.

```python
first_name = models.CharField("person's first name", max_length=30)
```

여기에서 Verbose name은 "person's first name"로 필드의 자세한 내용을 보여주는 것을 의미한다.


`ForeignKey`, `ManyToManyField`, `OneToOneField`는 첫 번째 인자로 모델 클래스를 가져야 하므로 `verbose name`인수를 사용한다.

```python
poll = models.ForeignKey(
    Poll,
    on_delete=models.CASCADE,
    verbose_name="the related poll",
)
sites = models.ManyToManyField(Site, verbose_name="list of sites")
place = models.OneToOneField(
    Place,
    on_delete=models.CASCADE,
    verbose_name="related place",
)
```

verbose_name을 사용할 때, 굳이 첫글자를 대문자로 할 필요는 없다. 장고에서 알아서 첫 글자를 대문자화 해준다.

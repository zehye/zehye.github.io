---
layout: post
title: Django - django-document 5, introduction to models (Model Inheritance / abstract base classes)
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

`django`에서는 세 가지 스타일의 상속을 제공한다.

1. 흔히 부모 클래스를 사용하여 각 하위 모델에 대해 일일이 입력하지 않으려는 정보를 제공하는 경우이다. 즉, 수많은 모델을 만들때, 공통적으로 사용되는 요소에 대해 다룰 때, 상속을 사용한다. 이 클래스는 따로 분리하여 사용하지 않으므로 추상 기본 클래스(Abstract base classes)를 사용한다.

생성일자와 수정일자만 적는 필드만 필요하다고 하면, 그 필드를 가지고 있는 부모 테이블이 있다면 그 부모테이블은 데이터 자체가 존재할 이유가 없다. 생성일자와 수정일자만 담고있으면 그 테이블은 존재할 이유가 없으니까.

그런 경우에는 부모 테이블을 생성하지 않고 이 테이블에 어떤 필드가 들어갈 것이다라는 정보만 제공하는 것

그래서 실제로 데이터베이스에 만들어지지 않는 경우를 abstract base class라고 한다.

2. 기존 모델을 하위 클래스 화(다른 애플리케이션의 모델이어도 무관)하고, 각 모델이 자체 데이터ㅂ이스 테이블을 가지기를 원한다면 다중 테이블 상속(Multi table inheritance)이 필요하다.

즉, 부모클래스도 어떤 데이터를 가지는데, 자기 자식 클래스(상속받은 클래스)도 따로 테이블을 가지는 것을 의미한다.

이 방식은 사실 너무 많이 쓰면 안된다. 이게 sql문에서 너무 성능이 안좋아지기도 하고, 한 테이블에서 다른 테이블의 정보를 가져오는 것은 연산이 많이 쓰이는 데, 이는 상속을 할때마다 테이블이 하나씩 더 생기기 때문에 만약에, 한 필드를 만들기 위해 5번의 테이블 상속을 만든다고 하면 파이썬 차원에서는 거의 자체에는 문제가 없는데 (파이썬 차원에서 클래스를 몇번 상속을 하든 성능상에는 문제가 없지만) 데이터 베이스에서는 굉장히 문제가 생긴다.

3. 마지막으로 모델 필드를 변경하지 않고 모델의 파이썬 수준 동작만 수정하려는 경우 `Proxy`모델을 사용할 수 있다.

Proxy 모델은 어떤 거쳐가는 역할을 한다는 의미(대응자)로 이는 데이터베이스 테이블은 한개인데, 그거에 대해서 우리는 장고, 혹은 파이썬 단에서 동작을 조금 제한하고 싶을때 혹은 메서드를 구분하고 싶을때 사용한다.

#### Abstract base classes

추상 기본 클래스는 몇가지 공통된 정보를 여러 다른 모델에 넣으려 할 때 유용하다. 이를 작성하기 위해서는 기본 클래스를 작성하고 `Meta`class에 `abstract = True`를 넣는다. 그러면 이 모델은 테이블을 만들어주지 않고 대신에 다른 모델에서 그 모델을 상속받을 때, 자식 내용 필드에만 해당 내용이 추가된다.

예시:
```python
class CommonInfo(models.Model):
  name = models.CharField(max_length=100)
  age = models.PositineIntegerField()

  class Meta:
    abstract = True

class Student(CommonInfo):
  home_group = models.CharField(max_length=5)
```

이렇게 `CommonInfo` 클래스 안에 `Meta` 클래스 `abstract=True`를 하고 나면, CommonInfo를 상속받은 Student는 home_group 필드만 적었음에도 name과 age 필드를 가져올 수 있다. 그런데 실제로 데이터베이스 테이블로 생성되는 것은 `Student`테이블 하나이다.

많은 경우, 이 유형의 모델 상속이 일반적이다.

#### Meta inheritance
추상 기본 클래스가 생성되면 Django는 기본 클래스에서 선언한 `Meta`내부 클래스를 속성으로 사용할 수 있게 한다. 자식 클래스가 자신의 메타 클래스를 선언하지 않으면 부모 클래스의 메타를 상속받는다.

사실 무슨 말인지 잘 모르겠으니 일단 예시를 보자.

```python
from django.db import models

class CommonInfo1(models.Model):
    name = models.CharField(max_length=50)

    class Meta:
        abstract = True
        ordering = ['-name']


class CommonInfo2(CommonInfo1):
  # CommonInfo1을 상속받았기 때문에 실제 name은 없어도 해당 필드가 데이터베이스 상에 존재한다.
    age = models.IntegerField(default=0)

    class Meta(CommonInfo1.Meta):
      # CommonInfo1를 상속받았는데, 또 abstract = True니까
      # CommonInfo1에 있던 필드가 CommonInfo2에 그대로 들어가면서
      # name과 age가 포함된 또다른 추상 클래스가 만들어진다.
        abstract = True
        verbose_name = 'CommonInfo2'


class User(CommonInfo1):
  # User는 username만 있지만 CommonInfo1를 상속받음으로써 name, username을 필드로 받는다.
    username = models.CharField(max_length=50)


class Student(CommonInfo2):
  # CommonInfo1와 CommonInfo2가 포함된 추상클래스를 받은 Student에는
  # name, age, cls 필드가 데이터베이스 상에 존재하게 된다.
    cls = models.CharField(max_length=50)
```
따라서 실제 데이터베이스를 보면 CommonInfo1과, CommonInfo2는 없고 `User`, `Student`만 존재한다.

`CommonInfo1`는 `abstract = True`옵션이 있으니까, 우리가 제일 처음으로 만든 `추상 기본 클래스`이다.

이 CommonInfo1를 상속받은 기본 클래스는 자동으로 name이라는 필드를 추가되어 있을 것이다. 그게 바로 User!

User의 경우 별도의 abstract = True 옵션이 없으니까, CommonInfo1를 상속받은 상태로 username필드 뿐만 아니라, name과 자식 클래스로 별도의 메타 클래스를 지정하지 않았으니까 ordering = ['-name']옵션까지 필드로 받아 데이터베이스 상태에 존재한다.

CommonInfo1라는 추상기본 클래스를 상속받아 또다른 추상 기본 클래스를 형성(abstract = True)하고 있는 `CommonInfo2`는 일단 기본적으로, name이라는 필드와 메타 옵션을 그대로 가져온다. 그런데 CommonInfo2에서는 메타옵션을 통해서 `재정의`를 하고 있다.

기본적으로 따로 설정을 하지 않으면 (단순 `class Meta:`를 하는 경우)부모를 무시하고 완전히 새로운 메타옵션을 만드는데, 지금의 경우는 부모의 메타 옵션을 그대로 가지고 오고 수정을 할 수 있도록 메타 옵션을 상속받았다. (`class Meta(CommonInfo1.Meta)`)

따라서 CommonInfo2는 CommonInfo1의 ordering = ['-name']을 가지고 오면서, verbose_name 까지 새로운 옵션으로 지정하게 되었다.

이러한 CommonInfo2를 상속받은 Student에는 CommonInfo2에서 만들어 놓은 옵션 뿐만 아니라 CommonInfo1에서 만든 옵션까지도 가지고 있는 것이다.

<hr>
일부 속성은 추상 기본 클래스의 `Meta`클래스에 포함하는 것이 타당하지 않다. 예로 들어, `db_table`을 포함하는 것은 모든 자식 클래스(자신의 메타를 지정하지 않은 클래스)가 동일한 데이터 베이스 테이블을 사용한다는 것을 의미하는데, 이는 대부분의 경우 원하지 않는 동작이다.
<hr>


**Be careful with related_name and related_query_name**

```python
from django.db import models

__all__ = (
    'RelatedUser',
    'PostBase',
    'PhotoPost',
    'TextPost',
)


class RelatedUser(models.Model):
    name = models.CharField(max_length=50)

    def __str__(self):
        return self.name


class PostBase(models.Model):
    author = models.ForeignKey(
        RelatedUser,
        on_delete=models.CASCADE,
        related_name='posts',
    )
    created_at = models.DateTimeField(auto_now_add=True)

    class Meta:
        abstract = True


class PhotoPost(PostBase):
    photo_url = models.CharField(max_length=500)


class TextPost(PostBase):
    text = models.TextField()
```
이렇게 `makemigrations`을 하고나면 에러가 뜰것이다.

```python
SystemCheckError: System check identified some issues:

ERRORS:
abstract_base_classes.PhotoPost.author: (fields.E304) Reverse accessor for 'PhotoPost.author' clashes with reverse accessor for 'TextPost.author'.
        HINT: Add or change a related_name argument to the definition for 'PhotoPost.author' or 'TextPost.author'.
abstract_base_classes.PhotoPost.author: (fields.E305) Reverse query name for 'PhotoPost.author' clashes with reverse query name for 'TextPost.author'.
        HINT: Add or change a related_name argument to the definition for 'PhotoPost.author' or 'TextPost.author'.
abstract_base_classes.TextPost.author: (fields.E304) Reverse accessor for 'TextPost.author' clashes with reverse accessor for 'PhotoPost.author'.
        HINT: Add or change a related_name argument to the definition for 'TextPost.author' or 'PhotoPost.author'.
abstract_base_classes.TextPost.author: (fields.E305) Reverse query name for 'TextPost.author' clashes with reverse query name for 'PhotoPost.author'.
        HINT: Add or change a related_name argument to the definition for 'TextPost.author' or 'PhotoPost.author'.
```
여기서 말하는 `Reverse accessor`와 `Reverse query name`은 뭘까?

query_name은 필터링할 떄 쓰는것, related_name은 objects로부터 역방향 manager를 꺼낼 때 쓰는 것


Reverse accessor는 역으로 db에 참조할 수 있게 도와주는 애로 이때의 accessor는 manager를 의미함으로, related_name인 posts를 의미한다. 즉, related_name에 지정된 name이 reverse accessor가 된다.

그리고 Reverse query name은 필터안에서의 쿼리 네임을 말하는 것으로 저 에러는 즉, related_name와 related_query_name이 겹친다는 의미이다.

이게 왜 겹치냐면, 지금 PhotoPost와 TextPost는 PostBase를 상속받고 있는데, 만약 PostBase에서 related_name을 지정해주지 않으면 자동으로 해당 클래스 네임을 기준으로 이름을 만들어준다. 그래서 RelatedUser이 PhotoPost를 역방향 참조할 때, related_name을 PhotoPost_set으로 지정이 되고, TextPost는 TextPost_set으로 지정이 된다.

그러면 분리가 되는데, 억지로 PostBase의 related_name='posts'라고 지어버리면 PhotoPost, TextPost 둘다 같은 author필드에 대한 related_name을 가지게 되는 것이다.

> 역방향 참조는 항상 unique해야하는데...

그래서 지금 ForeignKey 혹은 ManyToManyField를 abstract_base_classes에 지정을 했는데, 거기서 related_name을 따로 지정하고 싶다면 고유한 `reverse name`과 `reverse query name`을 지정해줘야 한다.

이 문제를 해결하려면 추상 기본 클래스에서 related_name또는 related_query_name을 사용할 때 값의 일부에 `%(app_label)s` 및 `%(class)s`를 포함해야 한다.

* `%(class)s`는 필드가 사용되는 하위 클래스의 lower-cased이름으로 대체된다.
  * 필드가 사용되는 하위클래스란, PhotoPost, TextPost에서의 author필드를 의미한다.

* `%(app_label)s`는 하위클래스가 포함된 애플리케이션 이름의 lower-cased이름으로 바뀐다.
  * 하위클래스가 포함된 애플리케이션 이름은 `abstract_base_classes` - 파이참에서 확인

한 애플리케이션 내에서 모델 클래스명은 고유해야하며, 애플리케이션의 이름또한 장고내에서 겹칠 수 없게 되어있다. (고유한 값)

```python
from django.db import models

__all__ = (
    'RelatedUser',
    'PostBase',
    'PhotoPost',
    'TextPost',
)


class RelatedUser(models.Model):
    name = models.CharField(max_length=50)

    def __str__(self):
        return self.name


class PostBase(models.Model):
    author = models.ForeignKey(
        RelatedUser,
        on_delete=models.CASCADE,
        # related_name='posts',  # 여기에 지정된 이름이 reverse accessor

        # abstract_base_classes_photopost
        # related_name='%(app_label)s_%(class)s'

        related_name='%(class)ss',
    )
    created_at = models.DateTimeField(auto_now_add=True)

    class Meta:
        abstract = True


class PhotoPost(PostBase):
    # author
    # related_name : photoposts
    photo_url = models.CharField(max_length=500)


class TextPost(PostBase):
    # author
    # relate_name : textposts
    text = models.TextField()
```

이렇게 고치면 각각의 고유한 relate_name을 만들어줄 수 있다!

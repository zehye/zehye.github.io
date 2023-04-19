---
layout: post
title: Django - 장고 모델에서 잊지말아야하는 중요한 tips!
category: Django
tags: [django]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.     
잘못된 내용이 있다면 편하게 댓글 남겨주세요!    

<hr>

## ForeignKey

Django에서 다대일 관계를 설정할 때는 `ForeignKey`를 사용한다.<br>
다른 필드 타입과 마찬가지로 모델클래스의 속성을 입력하며, 연결 대상이 될 모델 객체를 위치인자로 전달해주며 `on_delete`옵션을 필수로 입력해주어야 한다.

```python
class 모델이름(models.Model):
  필드이름 = models.ForeignKey(연결대상모델, on_delete=삭제옵션)
```

- 필드이름: 자유롭게 설정가능하지만, **연결대상 모델명의 소문자** 로 정하는 것을 권장한다.
- 재귀적관계: 한 테이블의 레코드들이 같은 테이블의 다른 테크드들과 관계를 형성하는것 `self`를 사용하여 설정가능하다.
- 아직 정의되지 않은 테이블과의 관계: 파이썬은 순서대로 코드를 읽어내려가기때문에 연결대상모델이 본 코드보다 아래에 있는 경우 참조를 하지 못한다.
  - `'모델명'`으로 적어준다.



## ManyToManyField

Django에서 테이블 간 다대다 관계를 설정하기 위해서는 `ManyToManyField`를 사용해 필드를 만들어준다.

```python
class 모델이름(models.Model):
  필드이름 = models.ManyToManyField(연결대상모델)
```

- 필드이름: 필드이름은 **복수형** 으로 설정하는 것을 권장한다.

- 중개모델 직접설정하기: 다대다관계에서는 두 테이블간의 관계를 표현하는 테이블이 자동 생성된다.
  - 하지만 사용자가 직접 생성도 가능하고 이때 추가적인 정보를 담은 필드들을 중개모델에 삽입 또한 가능하다. >> `through`

#### through, 중개모델과 중개모델의 제약사항

```python
class 모델이름(models.Model):
  필드이름 = models.ManyToManyField(연결대상모델, through='관계모델')
```

장고 문서에 있는 예시를 가져와보자.

```python
class Artist(models.Model):
    name = models.CharField(max_length=50)


class Band(models.Model):
    name = models.CharField(max_length=50)
    members = models.ManyToManyField(Artist, through='Membership')


class Membership(models.Model):
    artist = models.ForignKey(Artist, on_delete=models.CASCADE)
    band = models.ForignKey(Band, on_delete=models.CASCADE)
    is_founding_member = models.BooleanField()
```

중개모델을 직접 생성하는 경우, 관계를 가지는 두 테이블을 각각 참조하는 외래키 필드를 명확히 선언해주어야 한다.<br>
중개모델에는 다대다관계의 **소스모델(Source Model)** 과 **타겟모델(Target Model)** 을 참조하는 외래키가 반드시 각각 하나씩있어야 한다.

- 소스모델: `ManyToManyField` 필드가 있는 모델을 말한다.
- 타겟모델: `ManyToManyField` 에 인자로 전달되는 모델을 말한다.

만약 중개 모델에서 하나 이상의 외래키 필드가 소스모델 혹은 타겟모델을 참조한다면 `through_field`옵션을 통해 관계 형성에 사용할 외래키를 반드시 설정해주어야 한다.<br>
그렇지 않으면 `ValidationError`가 발생한다.

재귀적 다대다 관계를 맺을 경우, 중개 모델이 하나의 모델을 참조하는 두개의 외래키 필드를 가지는 것을 허용한다.<br>
그러나 같은 모델을 참조하는 외래키 필드가 두개를 초과하는 경우, 마찬가지로 `through_field`를 통해 관계 형성에 사용될 외래키 필드 두개를 명확히 지정해주어야 한다.

그리고 중개 모델을 직접 생성하여 재귀적 다대다 관계를 설정할 경우, `symatrical=False`옵션을 반드시 추가해주어야 한다.

```python
class 타겟모델(models.Model):
    필드1
    필드2
    ...


class 소스모델(models.Model):
    필드이름 = models.ManyToManyField(
        타겟모델,
        through=중개모델,
        through_fields=('소스필드', '타겟필드',)  # 반드시 소스필드, 타겟필드 순서로 된 튜플로 전달
        )
    필드2
    필드3
    ...


class 중개모델(models.Model):
    # through_fields 옵션에는 소스 및 타겟모델이 아닌 중개모델에 선언된 소스 및 타겟 "필드"의 이름을 문자열로 전달해야한다.
    타겟필드 = models.ForignKey(타겟모델)
    소스필드 = models.ForignKey(소스모델)
    추가외래키필드 = models.ForignKey(관계대상모델)
    ...
```

이때 `관계의역참조`가 발생하지 않기 위해 `related_name`을 지정해주는 것을 잊으면 안된다.<br>
외래키 필드를 가진 소스모델에 연결된 타겟모델의 인스턴스들은 자신과 연결된 소스모델의 인스턴스들을 가져올 수 있는 `Manager`를 갖는다.

기본적으로 Managersms `FOO_set`의 형태로 이름지어지며, 여기서 `FOO`는 소문자로 변환된 소스모델이름을 의미한다. <br>
`Reverse accessor`는 관계를 역참조할 수 있는 `Manager`를 가리킨다.

위의 Artist, Band, Membership 모델을 보면 현재 Band와 Membership모델은 Artist를 참조하고 있다.

만약 `membership_set`을 통해 `Artist`의 인스턴스가 참조되는 곳을 조회하도록하면 `Artist`모델을 참조하고 잇는 두 외래키 필드중 어느 필드의 값을 가져와야 하는지 알지 못한다.<br>
이때 `related_name`을 지정해주어야 한다.


## OneToOneField

한 테이블의 하나의 레코드가 다른 테이블의단 하나의 레코드만을 참조할 때, 이 두 모델간의 관계를 일대일 관계라고 한다.<br>
일대일관계는 어떤 테이블을 구조적으로 **확장** 시킬 때 유용하게 쓰인다.

OneToOneField는 ForignKey 필드에 `unique=True`옵션을 준 것과 동일하게 동작한다. 즉, 외래키필드의 값은 반드시 고유한 값이어야 한다.

```python
class 모델이름(models.Model):
  필드이름 = models.OneToOneField(관계대상모델)
```

- 소스모델이 타겟모델을 참조할때: 관계가 정의된 속성이름을 사용
- 타겟모델에서 소스모델을 역참조할때: `모델이름소문자_set`이 아닌 `소문자소스모델이름`을 사용

하나의 레코드는 단 하나의 레코드를 참조하기 때문에 하나의 모델 객체만을 돌려받는다. 

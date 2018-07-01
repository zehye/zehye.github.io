---
layout: post
title: Django - django-document 3, introduction to models (Meta options)
category: Django
tags: [django]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 django-document을 보고 정리했습니다.

<hr>

### Meta options

Meta data = data에 대한 data

아래와 같이 모델 클래스 내부에 Meta라는 이름의 클래스를 선언해서 모델에 메타데이터를 추가할 수 있다.

```python
from django.db import models

class Ox(models.Model):
  # 클래스 정의 자체가 하는 일은 테이블 안의 형태를 말한다.
  # 테이블 안에 어떤 컬럼이 있을 것이고 그 컬럼은 어떤 데이터를 담고 있을 것이다. 라는 것을 알려준다.
  horn_length = models.IntegerField()

  class Meta:
    # 이 모델 클래스에 정의된 Meta라는 이름을 갖는 클래스는 그 테이블 자체에 대한 데이터를 다시 나타낸다.
    ordering = ['horn_length']
    verbose_name_plural = 'oxen'
    # 위와 같이 정렬순서, 혹은 테이블이 나중에 관리자 페이지에서 어떻게 보일가에 대한 정보는
    # 필드와는 관련이 없고 이 클래스 전체, 테이블에만 관련있는 정보니까
    # 이를 meta라는 옵션에 넣어주면 된다.

    # 메타 옵션에 넣어주는 방법은 클래스 내부에 다시한번 클래스를 선언하면 된다.
```

> 클래스 안에 들어가 있는 필드는 어떤 데이터를 나타낼까? 필드가 있음으로 무언가를 나타내는데, 그게 무엇?

어떤 정보가 들어갈 수 있는지에 대해 알려주는 것은 필드의 타입이고, 필드 한개한개가 의미하는 것은 데이터베이스에서의 컬럼 하나이다.

(인스턴스에 정의되는 필드는 어떤 값을 가져온다.)

위 모델 클래스에 있는 테이블은 테이블 안에 있는 데이터들을 모아놓은 컬럼 정보를 나타내고
아래 메타 클래스는 테이블 자체의 데이터를 나타낸다. 즉 테이블 자제도 어떤 속성을 가질 수 있다.


모델 메타데이터는 앞에서 보았던 필드의 옵션과 달리, 모델 단위의 옵션이라고 볼 수 있다. 예를 들어, 정렬옵션(ordering), 데이터베이스 테이블 이름(db_table), 또는 읽기 좋은 이름 (verbose_name)이나 복수(verbose_name_plural)이름을 지정해 줄 수 있다.

모델 클래스에 Meta클래스를 반드시 선언해야 하는 것은 아니며, 또한 모든 옵션을 설정해야 하는 것도 아니다. 

---
layout: post
title: python 3.7부터 도입된 dataclass에 대해 알아보기
category: Python
tags: [python, iterable, iterator]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>


## dataclass

파이썬 3.7부터 표준 라이브러리로 등재된 `dataclass`에 대해 알아봅니다. (파이썬 3.6에서 데이터클래스를 사용하기 위해서는 `pip install dataclasses`를 해줬어야 했다.) 파이썬 3.7에서는 데이터클래스를 보다 용이하게 선언해주기 위해 데코레이터 `@dataclass`를 사용하면 되고 이를 사용하면 보다 간편하게 `__init__`,`__repr__` 등의 메소드를 자동으로 정의할 수 있다.(사용자 정의 클래스에 자동으로 추가할 수 있다.)


```python
from dataclasses import dataclass

@dataclass
class Book:
  title:str # 필드
  price:int = None

book = Book('당신은 데이터의 주인이 아니다...')
book
```

이 데이터클래스 데코레이터는 클래스를 검사하여 필드를 찾는다. 필드는 형 어노테이션을 가진 클래스 변수로 정의된다. 필드의 특징은 아래와 같다.

```python
@dataclass
class C:
  a: int  # 'a' has no default value
  b: int = 0  # assign a default value for 'b'
```

이렇게 기본값이 없는 필드는 항상 기본값이 있는 필드위에 정의되어야 한다. 그렇지 않으면 `TypeError`가 발생한다.

```python
rom dataclasses import dataclass

@dataclass
class Webtoon:
    # 이 클래스에서 공통적으로 사용할 변수는 클래스변수로 선언
    URL_WEBTOON_LIST = 'https://comic.naver.com/webtoon/weekday.nhn'
    URL_EPISODE_LIST = 'https://comic.naver.com/webtoon/list.nhn?titleId={id}'
    WEBTOON_LIST_HTML = None

    id: str
    url_thumbnail: str
    title: str

    def __post_init__(self):
        self.author = None
        self.description = None
        self.genres = None
        self.age = None

    def __repr__(self):
        # 객체의 표현값
        return f'Webtoon({self.title}, {self.id})'


```

---
layout: post
title: DRF tutorial 02. Serializer
category: DRF
tags: [drf]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## Serializer


### 환경설정

```
projects/drf/drf_tutorial
git init
.gitignore

  pyenv virtualenv 3.6.5 <가상환경 이름>
  pyenv local <가상환경 이름>
  pycharm interpreter 설정

  pipenv install django
  pipenv install djangorestframework
  pipenv install pygments

  django-admin startproject tutorial
    ./manage.py startapp snippets
```

#### INSTALLED APPS

```
INSTALLED_APPS = [
  'rest_framework',
  'snippets',
]
```

#### models.py
```python
from django.db import models
from pygments.lexers import get_all_lexers
from pygments.styles import get_all_styles

LEXERS = [item for item in get_all_lexers() if item[1]]
LANGUAGE_CHOICES = sorted([(item[1][0], item[0]) for item in LEXERS])
STYLE_CHOICES = sorted((item, item) for item in get_all_styles())


class Snippet(models.Model):
    created = models.DateTimeField(auto_now_add=True)
    title = models.CharField(max_length=100, blank=True, default='')
    code = models.TextField()
    linenos = models.BooleanField(default=False)
    language = models.CharField(choices=LANGUAGE_CHOICES, default='python', max_length=100)
    style = models.CharField(choices=STYLE_CHOICES, default='friendly', max_length=100)

    class Meta:
        ordering = ('created',)
```

```
./manage.py makemigration
./manage.py migrate
```


## serializer.py

```python
from django.forms import widgets
from rest_framework import serializers
from snippets.models import Snippet, LANGUAGE_CHOICES, STYLE_CHOICES


class SnippetSerializer(serializers.Serializer):
    pk = serializers.IntegerField(read_only=True)
    title = serializers.CharField(required=False, allow_blank=True, max_length=100)
    code = serializers.CharField(style={'base_template': 'textarea.html'})
    linenos = serializers.BooleanField(required=False)
    language = serializers.ChoiceField(choices=LANGUAGE_CHOICES, default='python')
    style = serializers.ChoiceField(choices=STYLE_CHOICES, default='friendly')

    def create(self, validated_data):
        """
        검증한 데이터로 새 `Snippet` 인스턴스를 생성하여 리턴합니다.
        """
        return Snippet.objects.create(**validated_data)

    def update(self, instance, validated_data):
        """
        검증한 데이터로 기존 `Snippet` 인스턴스를 업데이트한 후 리턴합니다.
        """
        instance.title = validated_data.get('title', instance.title)
        instance.code = validated_data.get('code', instance.code)
        instance.linenos = validated_data.get('linenos', instance.linenos)
        instance.language = validated_data.get('language', instance.language)
        instance.style = validated_data.get('style', instance.style)
        instance.save()
        return instance
```

serializer는 django Form 같은 경우에는 form데이터를 받아 처리하는 능력이 있는데 serializer는 json데이터를 받아 처리할 수 있는 능력이 있다.

이 서버로 json데이터를 보내면 serializer를 통해 데이터를 받으면 그 데이터가 제대로 들어왔는 지 `is_valid`, 혹은 객체를 만들어줄 수도 있다. validater data에 접근한다던가..

### 시리얼라이저 사용하기

```
./manage.py shell_plus
```

```shell_plus
from snippets.serializer import SnippetSerializer

# renderers는 serializer를 저장할때 객체를 변환하는 것을 뜻한다.
from rest_framework.renderers import JSONRenderer

# 이미 serializer되어있는 데이터를 다시 객체로 만드는 것을 parser
from rest_framework.parsers import JSONParser


snippet = Snippet(code='foo = "bar"\n')
snippet.save()
snippet = Snippet(code='print "hello world"\n')
snippet.save()

serializer = SnippetSerializer(snippet)

serializer.data
>>> {'pk': 2, 'title': '', 'code': 'print "hello world"\n', 'linenos': False, 'language': 'python', 'style': 'friendly'}

content = JSONRenderer().render(serializer.data)
content
>>> b'{"pk":2,"title":"","code":"print \\"hello world\\"\\n","linenos":false,"language":"python","style":"friendly"}'
```

이때의 b는 bytes string으로 인코딩 되어있지 않은 데이터로 이를 문자열 형태로 저장할 수 있다. JSONParser를 통해서

```shell_plus
# 파일객체라는 것 자체가 연속적인 데이터를 다룰 수 있게 해준다 (stream data)
# 파일로 존재하는 데이터 모두 연속적인 데이터로 파일 객체는 이들을 불러올 수 있다.
# ByptesIO는 메모리 상에 존재하는 어떤 연속적인 데이터를 파일처럼 다룰 수 있게 해준다.
from django.utils.six import ByptesIO

stream = ByptesIO(content)
stream
>>> <_io.BytesIO object at 0x110158ca8>

data = JSONParser().parse(stream)
data
>>> {'pk': 2, 'title': '', 'code': 'print "hello world"\n', 'linenos': False, 'language': 'python', 'style': 'friendly'}
```

이때 data와 serializer의 값이 같은데, 어찌됐든 메모리상의 데이터는 다르다.

```shell_plus
type(data)
>>> <class 'dict'>

type(serializer.data)
>>> class 'rest_framework.utils.serializer_helpers.ReturnDict'>
```

물론 type도 다르다.

```python
snippet2 = Snippet(**serializer.data)
```

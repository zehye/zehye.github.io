---
layout: post
title: Django - makemigrations 취소하는 방법
category: Django
tags: [python, pycharm, django]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

장고를 만지다보면 자연스럽게 마이그레이션을 실수하는 경우가 생긴다.

이러한 경우에 할 수 있는 migrations을 지우고 해제하는 방법에 대해 정리해본다.

**데이터베이스가 이상하면 이전 상태로 돌아간 다음에 migrations을 지워야 한다.**

shell에 들어가서 `~/manage.py showmigrations`

```python
abstract_base_classes
 [X] 0001_initial
 [X] 0002_auto_20180619_0536
```
이런식으로 나올 것이다.

우리가 현재 migrations한 상황들을 볼 수 있고, `migrate`를 통해서 현재 상태를 어디로 돌아갈 것인지를 정할 수 있다.

우리가 현재 abstract_base_classes의 0001번으로 돌아가고 싶다면,

`~/manage.py migrate abstract_base_classes 0001`

```python
Operations to perform:
  Target specific migration: 0001_initial, from abstract_base_classes
Running migrations:
  Rending model stated... DONE
  Unapplying abstract_base_classes.00020002_auto_20180619_0536...OK
```
적용이 해지가 된 것을 볼 수 있다.

그러고 다시 `~/manage.py showmigrations`을 해보면
```python
abstract_base_classes
 [X] 0001_initial
 [ ] 0002_auto_20180619_0536
```
그리고 이러한 상태에서는 파이참 migrations 폴더에 있는 파일을 직접 delete할 수 있다.

데이터베이스가 이상하다고 해서 migrations에 있는 파일을 지우면 안되는 이유가 여기에 있다.

---
layout: post
title: python 모르는 부분 따로 정리
category: python
tags: [python, enumerate, index, arguments]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## index 함수

한 리스트 내에서 내가 원하는 valuew의 위치를 알고 싶을 때 사용하는 함수

기본 사용법

```python
A = [0,1,2,3]

target = 1
A.index(target)
>>> 1
```

=> index()는 찾는 값의 첫번째 위치를 반환한다


## enumerate

enumerate는 열거하다라는 뜻으로 순서가 있는 자료형(리스트, 튜플, 문자열)을 입력으로 받아 인덱스 값을 포함하는 enumerate 객체를 리턴한다.

(보통 for문과 자주 사용된다.)

```python
for i, name in enumerate(['body', 'foo', 'bar']):
... print(i, name)
...
>>> 0 body
    1 foo
    2 bar
```

순서값과 함께, body, foo, bar가 순서대로 출력되었다. 즉, 위 예제와 같이 enumerate를 for문과 함께 사용하면 자료형의 현재 순서(index)와 그 값을 쉽게 알 수 있다.

for 문처럼 반복되는 구간에서 객체가 현재 어느 위치에 있는 지 알려주는 인덱스 값이 필요할 때 사용하면 매우 유용하다.


## dictinary key로 value얻기 (get)

```python
>>> a = {'name':'pey', 'phone':'0119993323', 'birth': '1118'}
>>> a.get('name')
'pey'
>>> a.get('phone')
'0119993323'
```

get(x) 함수는 x라는 key에 대응되는 value를 돌려준다. 앞서 살펴보았듯이 a.get('name')은 a['name']을 사용했을 때와 동일한 결과값을 돌려받는다.

다만, 다음 예제에서 볼 수 있듯이 a['nokey']처럼 존재하지 않는 키(nokey)로 값을 가져오려고 할 경우 a['nokey']는 Key 오류를 발생시키고 a.get('nokey')는 None을 리턴한다는 차이가 있다. 어떤것을 사용할지는 여러분의 선택이다.

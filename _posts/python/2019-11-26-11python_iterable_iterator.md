---
layout: post
title: python iterable과 iterator
category: python
tags: [python, iterable, iterator]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## Iterable과 Iterator

파이썬을 공부하면 할수록 용어의 어려움을 계속해서 느끼고있다. 파이썬 뿐만 아니라 일반적인 프로그래밍 언어에는 `iterator`라는 개념이 있는데, 종종 `iterable`이라는 개념과 혼동되는 경우가 많았다.


### Iterable

- 반복 가능한 객체
- iterator로 변환 가능한 객체
- 대표적인 iterable한 타입: list, dict, set, str, tuple, range...

여기서 iterator로 변환이 가능하다는 말은, 값을 한 개씩 순차적으로 접근이 가능하도록 만들 수 있다는 것을 의미한다. 좀 더 쉽게 설명하면 for문에 넣어 돌리는 것이 가능하다는 것이다.

```python
li = [1,2,3]

print(li)  # [1,2,3]
print(li[1])  # [2]

next(li)  # 'list' object is not an iterator
```

위 예시에서도 볼 수 있듯 list를 print를 통해 한번에 여러개 값이 반환가능한 것은 발견했지만, next 메소드를 써서 한번에 한개씩 순차적으로 접근하려고 보니 list객체는 iterator가 아니라는 에러를 발생했다.

```python
li = [1,2,3]
li_iter = iter(li)

next(li_iter)  # 1
next(li_iter)  # 2
next(li_iter)  # 3
next(li_iter)  # StopIteration

type(li_iter)  # <class 'list_iterator'
```

이렇게 iter 메소드를 통해 순차적으로 접근이 가능해진 것을 볼 수 있었다. 그리고 li_iter의 타입을 확인해보니 `list_iterator`라는 iterator로 변환된 것을 볼 수 있었다. 즉, list는 iterator로 변환이 가능하기에 iterable한 객체인 것이다.


```python
li = [1,2,3]
for item in li:
  print(item)  # 1,2,3
```

이렇게 for문을 돌려봄으로써도 확인할 수 있듯 for문에 list를 넣어 돌리면 순차적인 접근이 가능하다. 명시적으로 list를 iterator로 바꾸어주지 않았음에도 불구하고 말이다. 이는 for문에서 내부적으로 list를 iterator로 바꾸어 순차접근을 하고 있기 때문이다. 즉, for문을 돌려 순차접근이 가능한 객체는 모두 iterable하다고 볼 수 있다.



## Iterator

- 값을 차례대로 꺼낼 수 있는 객체
- 파이썬 내장함수 `iter()`를 통해 iterator 객체를 만든다.



```python
li = [1,2,3]
li_iter = iter(li)

next(li_iter)  # 1
next(li_iter)  # 2
next(li_iter)  # 3
next(li_iter)  # StopIteration

type(li_iter)  # <class 'list_iterator'
```

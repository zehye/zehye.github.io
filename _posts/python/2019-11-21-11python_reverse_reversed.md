---
layout: post
title: python reverse, reversed 함수의 차이
category: Python
tags: [python, reversed, reverse]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## reverse()

- list의 자료형 함수 = list 타입에서 제공하는 함수
- list 요소를 역순으로 정렬해준다.


```python
l = [1,2,3]  #[3,2,1]
t = (1,2,3)  # AttributeError: 'tuple' object has no attribute 'reverse'
d = {'a':1, 'b':2, 'c':3}  # AttributeError: 'dict' object has no attribute 'reverse'
s = '123'  # AttributeError: 'str' object has no attribute 'reverse'
```

```python
li = [1,2,3]
li_reverse = li.reverse()
print(li_reverse)  # None, 값을 반환해주지 않고 단순히 해당 list를 뒤섞어준다.
print(li)  # [3,2,1]
```


## reversed()

- 파이썬의 내장함수 = list에서 제공해주는 함수가 아니다.
- iterator의 요소를 역순으로 리턴
- 원본을 변경하지는 않는다.

```python
l = [1,2,3]  # <listreverseiterator object at 0x101053c10>
t = (1,2,3)  # <reversed object at 0x101053b50>
d = {'a':1, 'b':2, 'c':3}  # TypeError: argument to reversed() must be a sequence
s = '123'  # <reversed object at 0x101053c10>
```

이렇게 딕셔너리는 시퀀스 타입이 아니기에 지원해주지 않으며, 인덱스로 순차적인 접근이 가능하지 않은 애들한테는 reversed()는 지원되지 않는다. 그리고 reversed함수는 `reversed객체`를 반환한다. 근데 list에만 `listreverseiterator`를 반환하고 있다. (아직 이 유의미한 차이에 대해서는 잘 모르겠다...)

여튼, reversed객체는 list나 tuple로 사용하고 싶다면 아래와 같이 한다.

```python
li = [ i for i in range(5) ]
print(list(reversed(li)))  # [4,3,2,1,0]
print(li)  # [0,1,2,3,4]
```

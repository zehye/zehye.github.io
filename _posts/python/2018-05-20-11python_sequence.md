---
layout: post
title: python 시퀀스(list, tuple)
category: Python
tags: [python, list, tuple]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 `python` 시퀀스에 대해 설명합니다.

주요 용어 :  `index` `slice` `append` `extend` `insert` `del` `remove` `pop` `count`

<hr>

## 시퀀스타입

파이썬에 내장된 시퀀스 타입에는 문자열, 리스트, 튜플이 있다.

문자열은 인용부호('',"")를 사용하며, 리스트는 대괄호[], 튜플은 괄호()를 사용하여 나타낸다.


### 리스트

```python
>>> empty_list1 = []
>>> empty_list2 = list()
>>> sample_list = ['a', 'b', 'c', 'd']
>>> sample_list2 = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
```

**인덱스 연산**

* sample_list2를 이용해서 실습. 5월, 7월을 인덱스연산을 통해 추출해보자.

```python
sample_list2[4]
sample_list2[6]
```


**내부항목 변경**
* sample_list를 이용, 3번째 요소인 'c'를 대문자 'C'로 바꿔본다.

```python
sample_list[2] = C

```

**슬라이스 연산**
* sample_list2를 이용, 1월부터 3월씩 건너뛴 결과를 quarters에 할당

```python
qurters = sample_list2[::3]
```

* sample_list2를 이용, 끝에서부터 3번째 요소까지를 last_three에 할당

```python
last_three = sample_list2[:-4:-1]
```

* sample_list2를 이용, 끝에서부터 처음까지(거꾸로) 2월씩 건너뛴 결과를 reverse_two_steps에 할당

```python
reverse_two_steps = sample_list2[::-2]
```

### 리스트 항목 추가 (append)

```python
>>> sample_list.append('e')
>>> sample_list
['a', 'b', 'c', 'd', 'e']
```

### 리스트 병합 (extend, +=)

```python
>>> fruits = ['apple', 'banana', 'melon']
>>> colors = ['red', 'green', 'blue']
>>> fruits.extend(colors)
>>> fruits
['apple', 'banana', 'melon', 'red', 'green', 'blue']
```

```python
>>> fruits = ['apple', 'banana', 'melon']
>>> colors = ['red', 'green', 'blue']
>>> fruits += colors
>>> fruits
['apple', 'banana', 'melon', 'red', 'green', 'blue']
```

> extend대신 append를 사용하면?

> fruits.append(color) 리스트 안에 리스트를 추가하는 모습이 나온다.

`['apple', 'banana', 'melon', ['red', 'green', 'blue']]`

### 특정 위치에 리스트 항목 추가 (insert)

* fruits리스트의 1번째 위치에 'mango'를 추가해보자

```python
fruits.insert(1, 'mango')
```

* fruits리스트의 100번째 위치에 'pineapple'을 추가해보자

```python
fruits.insert(100, 'pineapple')
```

### 특정 위치 리스트 항목 삭제 (del)

`del fruits[0]`

### 값으로 리스트 항목 삭제 (remove)

`fruits.remove('mango')`

### 리스트 항목 추출 후 삭제 (pop)

```python
fruits.pop()
fruits.pop(-3)
```

### 값으로 리스트 항목 찾기 (index)

```python
>>> fruits.index('apple')
0
```

### 존재여부 확인 (in)

`'apple' in fruits`

값은 True, False로 나온다.

### 값 세기 (count)

```python
>>> fruits.append('red')
>>> fruits.append('red')
>>> fruits.count('red')
3
```

### 정렬하기 (sort, sorted)

* sort는 리스트 자체를 정렬한다.
* sorted는 리스트의 정렬 복사본을 반환

```python
def number(n):
  # number라는 함수는 매개변수 n의 1번째 요소를 리턴해준다.
  return n[1]

random = [(1,2,3), (2,3,4), (1,4,2), (2,1,3),(2,5,6)]
# random이라는 변수를 함수 number에 따라 정렬한다.
random.sort(key=number)
print(random)
>>> [(2, 1, 3), (1, 2, 3), (2, 3, 4), (1, 4, 2), (2, 5, 6)]
```

### 튜플 (tuple)

리스트와 비슷하나, 정의 후 내부 항목의 삭제나 수정이 불가능하다.

개발을 하다보면 변하지 않아야 하는 값들이 있는데, 그런 값들은 튜플에 넣어주면 유용하게 쓸 수 있다.




### 실습
1.문자열 'Fastcampus'를 리스트, 튜플 타입으로 형변환하여 새 변수에 할당한다.

```python
li = ['Fastcampus,']
tu = ('Fastcampus,')
```

---
layout: post
title:  python 시퀀스
categories: [python]
---
이 포스팅에서는 `pyrhon`의 시퀀스에 대해 공부해본다.

<hr>

## 시퀀스타입

파이썬에 내장된 시퀀스 타입에는 문자열, 리스트, 튜플이 있다.

문자열은 인용부호('',"")를 사용하며, 리스트는 대괄호[], 튜플은 괄호()를 사용하여 나타낸다.


### 리스트

```
>>> empty_list1 = []
>>> empty_list2 = list()
>>> sample_list = ['a', 'b', 'c', 'd']
>>> sample_list2 = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
```

**인덱스 연산**

* sample_list2를 이용해서 실습. 5월, 7월을 인덱스연산을 통해 추출해보자.

```
sample_list2[4]
sample_list2[6]
```


**내부항목 변경**
* sample_list를 이용, 3번째 요소인 'c'를 대문자 'C'로 바꿔본다.

```
sample_list[2] = C

```

**슬라이스 연산**
* sample_list2를 이용, 1월부터 3월씩 건너뛴 결과를 quarters에 할당

```
qurters = sample_list2[::3]
```

* sample_list2를 이용, 끝에서부터 3번째 요소까지를 last_three에 할당

```
last_three = sample_list2[:-4:-1]
```

* sample_list2를 이용, 끝에서부터 처음까지(거꾸로) 2월씩 건너뛴 결과를 reverse_two_steps에 할당

```
reverse_two_steps = sample_list2[::-2]
```

### 리스트 항목 추가 (append)

```
>>> sample_list.append('e')
>>> sample_list
['a', 'b', 'c', 'd', 'e']
```

### 리스트 병합 (extend, +=)

```
>>> fruits = ['apple', 'banana', 'melon']
>>> colors = ['red', 'green', 'blue']
>>> fruits.extend(colors)
>>> fruits
['apple', 'banana', 'melon', 'red', 'green', 'blue']
```

```
>>> fruits = ['apple', 'banana', 'melon']
>>> colors = ['red', 'green', 'blue']
>>> fruits.extend(colors)
>>> fruits
['apple', 'banana', 'melon', 'red', 'green', 'blue']
```

> extend대신 append를 사용하면?
fruits.append(color)
리스트 안에 리스트를 추가하는 모습이 나온다.

`['apple', 'banana', 'melon', ['red', 'green', 'blue']]`

### 특정 위치에 리스트 항목 추가 (insert)

* fruits리스트의 1번째 위치에 'mango'를 추가해보자

```
fruits.insert(1, 'mango')
```

* fruits리스트의 100번째 위치에 'pineapple'을 추가해보자

```
fruits.insert(100, 'pineapple')
```

### 특정 위치 리스트 항목 삭제 (del)

`del fruits[0]`

### 값으로 리스트 항목 삭제 (remove)

`fruits.remove('mango')`

### 리스트 항목 추출 후 삭제 (pop)

```
fruits.pop()
fruits.pop(-3)
```

### 값으로 리스트 항목 찾기 (index)

```
>>> fruits.index('apple')
0
```

### 존재여부 확인 (in)

`'apple' in fruits`

값은 True, False로 나온다.

### 값 세기 (count)

```
>>> fruits.append('red')
>>> fruits.append('red')
>>> fruits.count('red')
3
```

### 정렬하기 (sort, sorted)

* sort는 리스트 자체를 정렬한다.
* sorted는 리스트의 정렬 복사본을 반환


### 튜플 (tuple)

리스트와 비슷하나, 정의 후 내부 항목의 삭제나 수정이 불가능하다.

개발을 하다보면 변하지 않아야 하는 값들이 있는데, 그런 값들은 튜플에 넣어주면 유용하게 쓸 수 있다.




### 실습
1.문자열 'Fastcampus'를 리스트, 튜플 타입으로 형변환하여 새 변수에 할당한다.

```
li = ['Fastcampus,']
tu = ('Fastcampus,')
```

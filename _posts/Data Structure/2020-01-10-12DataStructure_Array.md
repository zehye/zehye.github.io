---
layout: post
title: Data Structure, Abstract data types
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>


## Array

동일한 데이터를 index를 활용하여 저장, 접근할 수 있는 것

- 각 요소들에게는 index가 붙어져 있어야 한다.
- 배열의 길이만큼은 순회한다.
- 배열안 데이터의 유/무를 알아보기 위해서는 최소한 N회 루프를 돌아야한다.



### Insert procedure in array

1. 새로 들어갈 데이터가 포함된 공간을 생성
2. 레퍼런스 카피 > 저장할 공간 직전까지
3. 새로 만든 공간에 추가하고 싶은 데이터 저장
4. 이전의 레퍼런스 카피해서 연결


<center>
<figure>
<img src="/assets/post-img/DataStructure/1.jpeg" alt="" width="80%">
</figure>
</center>


```python
x = ['a', 'b', 'c', 'd', 'e', 'f']

idxInsert = 2
valInsert = 'c'

y = range(6)

for i in range(0, idxInsert):
  y[i] = x[i]
  y[idxInsert] = valInsert

for i in range(idxInsert, len(x)):
  y[i+1] = x[i]

x=y
```
y에 새로운 리스트가 만들어졌으니 `x=y`를 함으로써 y의 배열(레퍼런스)구조를 x에 저장!


#### 그렇다면 몇번의 operation이 필요했는가?

array를 이용해 무언가를 넣을때는 **n번** 의 operation이 필요하다.



### Delete procedure in array

1. 하나 줄어든 길이의 저장공간을 만든다.
2. 삭제하려고 하는 이전 값들은 그대로 카피한다.
3. 없애는 operation은 아무것도 안한다.
4. 삭제할 위치의 +1 값은 -1해준 위치에서 저장한다.


<center>
<figure>
<img src="/assets/post-img/DataStructure/2.jpeg" alt="" width="80%">
</figure>
</center>


```python
idxDelete = 3
y = range(5)

for i in range(0, idxDelete):
  y[i] = x[i]

for i in range(idxDelete+1, len(x)):
  y[i-1] = x[i]

x=y
```

array를 이용해 무언가를 넣을때는 **n-1번** 의 operation이 필요하다.


## Problem in Array

- 무엇이든지 넣거나 뺄때 N개의 검색이 필요하다.
  - 즉 배열을 처음부터 끝까지 다 건들여야 insert, delete가 가능하다는 의미이다.

현실적으로 불가능.

**어떤 마법이 있어서 줄 사이에 데이터가 들어갈만한 공간이 자동으로 만들어진다면?**<br>
배열 중간에 공간을 만들면 되지만 이는 1차원 적인 선형 공간이 아니기 때문에 더이상 배열이라고 할수가 없다. <br>
즉, 선형 공간을 그대로 유지한다면 공간을 만드는게 불가능하다.

---
layout: post
title: Data Structure, Bubble sort algorithm
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

- Pseudo code: 사람이 쓰는말로 명시되어있지만 컴퓨터의 프로그램을 짜기위해 사람이 이해하기 위해 짜놓은 알고리즘
  - 어떤 프로그램 언어에 국한되지는 않지만 프로그램을 짤 수 있을정도로 잘 명시된 코드

## Bubble sort algorithm

1. list를 하나 받는다.
2. for문을 통해 오퍼레이션을 수행한다.

```python
import random

n = 10
number = range(n)
random.shuffle(number)

def performSelecrionSort(number):
  for itr1 in range(0,n):
    for itr2 in range(itr1+1, n):
      if number[itr1] < number[itr2]:
        number[itr1], number[itr2]
      else:
        number[itr2], number[itr1]
  return number
```

- Total iterations: 2/1(n(n-1))

이렇듯 프로그램이 주어지면 이를 수학적으로 분석해볼 수 있다는 것을 알 수 있다. > 효율성을 생각해보자

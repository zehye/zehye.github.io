---
layout: post
title: Data Structure, Recursion
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Recursion, Repeating problems and Divide and Conquer

재귀호출(자기자신에게 다시 돌아온다)

- Repeating problems
  - 큰 하나의 문제가 정의되면 그 문제를 쪼개서 다시 반복되는 또다른 문제를 만드는것 > 다만 그 문제를 작아지게끔 만든다.

- Divide and Conquer: 문제를 쪼개어서 그 문제를 해결해나가는 것

```python
class Department:
  dept = [sales, manu, randd]
  def calculateBudget(self):
    sum = 0
    for itr in range(0, numDepartments):
      sum = sum + dept[itr].calculateBudget()
    return sum
```

1. calculateBudget() 동일한 함수를 호출하는데, 다만 분화된 작은 문제로 동일한 함수로 호출
2. Repeating problems를 프로그램으로 풀어보는 과정 > 그 컨셉을 Divide and Conquer로 풀어보는 것을 의미

#### 예시 - Factorial

<center>
<figure>
<img src="/assets/post-img/DataStructure/13.jpeg" alt="" width="80%">
</figure>
</center>


```
Factorial(n) = n x (n-1) x ... x 2 x 1
Repeating problems Factorial = n x Factorial(n-1)
```

#### 예시 - Great Common Divison, 최대공약수

```
Euclid's algorithm
GCD(A,B) = GCD(B, A mod B)
GCD(A,0) = A

GCD(32, 24)
= GCD(24, 8)
= GCD(8,0)
```

**공통점** <br>
1. 함수 호출은 반복된다.
2. 파라미터는 줄어든다
3. 수학적 귀납법과 유사한 프로그래밍이다.


## Recursion

함수를 호출하는데 그 함수 안에서 호출하는 것

Psuedo code를 확인해보자.

```python
def functionA(target):
  ...
  functionA(target1)  //size가 줄어든 target parameter
  ...
  if (escapeCondition):  
    return A
```

실제 코드로 확인해보자.

```python
def Fibonacci(n):
  if n == 0:
    return 0
  if n == 1:
    intRet = Fibonacci(n-a) + Fibonacci(n-2)
    return intRet

  for itr in range(0, 10):
    print(Fibonacci(itr),)
```


### Recursions and Stackframe

<center>
<figure>
<img src="/assets/post-img/DataStructure/14.jpeg" alt="" width="80%">
</figure>
</center>

R.A: Return Address, 어떤 함수가 호출되었는지 저장. 그래서 다음에 함수 호출한 위치로 돌아갈 수 있도록 도와줌

함수속에 함수가 호출되는 것이 계속 반복적으로 일어나는 것, 이 함수 호출은 컴퓨터 내부에서 Stackframe에서 아이템을 증가시켜준다.

Stackframe은 stack인데, 함수 호출의 역사를 기록하고 있다 > push, pop을 정의해야한다.

push: 함수가 호출되면 실행<br>
pop: 함수가 리턴을 함으로써 밖으로 나가게 될 떄 pop 실행

무엇을 저장? > 지역 변수(함수 안에서만 접근가능한 변수)와 함수호출 파라미터(함수호출에 할당된 파라미터)


### Merge sort

**Recursion을 이용한 sorting algorithm**

기본적인 원리는 더 작은 리스트사이즈로 쪼개고 나서 더이상 쪼갤 수 없는 한 숫자가 되었을 떄 비교해가면서 리스트를 합쳐나가는 과정

<center>
<figure>
<img src="/assets/post-img/DataStructure/15.jpeg" alt="" width="80%">
</figure>
</center>

실제 코드로 확인해보자.

```python
import random

def performMergeSort(firstElementToSort):
  if len(firstElementToSort) == 1:
    return firstElementToSort

  firstsubElementTisSort1 = []
  firstsubElementTisSort2 = []

  for itr in range(len(firstElementToSort)):
    if len(firstElementToSort) / 2 > itr:
      firstsubElementTisSort1.append(firstElementToSort[itr])
    else:
      firstsubElementTisSort2.append(firstElementToSort[itr])

    firstsubElementTisSort1 = performMergeSort(firstsubElementTisSort1)
    firstsubElementTisSort2 = performMergeSort(firstsubElementTisSort2)

    idxCount1 = 0
    idxCount2 = 0

    for itr in range(len(firstElementToSort)):
      if idxCount1 == len(firstsubElementTisSort1):
        firstElementToSort[itr] = firstsubElementTisSort2[idxCount2]
        idxCount2 = idxCount2 + 1
      elif idxCount2 == len(firstsubElementTisSort2):
        firstElementToSort[itr] = firstsubElementTisSort1[idxCount1]
        idxCount1 - idxCount1 + 1
      elif firstsubElementTisSort1[idxCount1] > firstsubElementTisSort2[idxCount2]:
        firstElementToSort[itr] = firstsubElementTisSort2[idxCount2]
        idxCount2 = idxCount2 + 1
      else:
        firstElementToSort[itr] = firstsubElementTisSort1[idxCount1]
        idxCount1 = idxCount1 + 1
      return firstElementToSort
```

### problems in Recursion of Fibonacci Seqeunce

- 함수 호출이 너무 많다 > 예전에 호출했던 애들이 같은 파라미터로 계속해서 호출이 된다.

즉, 불필요한 시간과 메모리 공간을 잡아먹게 된다. 이를 해결하기 위해 Dynamic Programming을 활용하여 너무 많은 함수 호출을 없앤다. > 한번 콜한 것을 기록해둔다.

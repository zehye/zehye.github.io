---
layout: post
title: Data Structure, Fibonacci Seqeunce in Dynamic Programming
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Fibonacci Seqeunce in Dynamic Programming

이전에는 Fibonacci를 recursion으로 풀었는데, 이는 함수내에 그 함수를 호출해서 풀었지만 DP에서는 그런 방법은 없다.

```python
def FibonacciDP(n):
  dicFibonacci = {}
  dicFibonacci[0] = 0
  dicFibonacci[1] = 1

  for itr in range(2, n+1):  # Building up a bigger solitions
    dicFibonacci[itr] = dicFibonacci[itr-1] + dicFibonacci[itr-2]
  return dicFibonacci[n]

for itr in range(0,10):  # Execution part
  print(Fibonacci(itr),)
```

딕셔너리를 이용하면 매우 간편하게 다이나믹 프로그래밍의 Memoization table을 짤 수 있다.

딕셔너리 테이블은 >> **O(N)** 

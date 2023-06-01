---
layout: post
title: Data Structure, Big-Oh notation과 Growth rate
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Big-Oh notation: Asymptotic notation

Notation to show the worst case running time! >  예) if가 항상 True인경우


### Definition of the Big-Oh notations

• f(N) = O(g(N))<br>
• There are positive constants c and n0 such that <br>
• f(N) ≦ c g(N) when N ≧ n0<br>
• The growth rate of f(N) is less than or equal to the growth rate of g(N)<br>
• g(N) is an upper bound


## Growth rate

<center>
<figure>
<img src="/assets/post-img/DataStructure/35.jpeg" alt="" width="50%">
</figure>
</center>


### Example of Big-Oh notation


- Assume f(N) = 7N2
  - f(N) = O(N4)
  - f(N) = O(N3)
  - f(N) = O(N2) (best answer, asymptotically tight)

- N2 / 2 – 3N -> O(N2)
- 1 + 4N -> O(N)
- 7N2 + 10N + 3 -> O(N2)
- log10 N = log2 N / log2 10 -> O(log2 N) = O(log N)
- sin N -> O(1)
- 10 -> O(1)
- logN + N -> O(N)


### Rules of Big-Oh notation

- growth rate이 가장 큰 것을 찾는것이 중요하다 > c라는 계수는 우리가 임의로 정해줄 수 있기 때문!
- log의 베이스는 적어주지 않아도 된다 > c를 밑으로 내려서 계수 취급을 할 수 있기 때문!

- If T1(N) = O(f(N)) and T2(N) = O(g(N)), then
  - T1(N) + T2(N) = max(O(f(N)),  O(g(N)))
    - max(O(N), O(N2)) = O(N2)
  - T1(N) * T2(N) = O(f(N) * g(N))
    - O(N) * O(logN) = O(NlogN)


### Big-Oh notation of list, stack, queue

<center>
<figure>
<img src="/assets/post-img/DataStructure/36.png" alt="" width="50%">
</figure>
</center>


### Performance of binary search tree

<center>
<figure>
<img src="/assets/post-img/DataStructure/37.png" alt="" width="50%">
</figure>
</center>

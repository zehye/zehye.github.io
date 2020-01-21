---
layout: post
title: Data Structure, Dynamic Programming
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Dynamic Programming >> planning

Recursion에 의해 나오는 다양한 함수호출 결과를 저장, 재활용함으로써 실행시간을 빠르게 하는것

<center>
<figure>
<img src="/assets/post-img/DataStructure/13.jpeg" alt="" width="80%">
</figure>
</center>

- overlapping sun-instance: F4를 구하기 위해 F2, F1(sub-instance)들이 overlapping하고 있음


### 어떻게 이루어지는가?

1. Setting up recurrence
2. Relating a solution if a large instance to solutions of some smaller instances
3. Solve small instances once
4. Record solutions in s table
5. Extract a solution of a larger instance from the table



### Memoization

기존의 함수 호출과 결과를 재활용하기 위해 저장하는 것

Memoization과 이전에 Recursion을 돌아가기 위해 만든 Stackframe 사이에는 상반된 철학적 차이가 있다.

1. Memoization: 밑에서부터 올라가는 접근. 언제 F4에 접근할 수 있을지는 모르며 과거에 풀어왔던 것을 기록해가며 결과를 도출해나가는 것
2. Recursion Stackframe: 위에서부터 아래로 접근. 풀려고 하는 목표를 먼저 실행하고 sub-instance들을 call

접근방법이 완전히 다르다.

Recursion이 overlapping sub-instance가 많다고 하면 Dynamic Programming을 이용해 문제를 풀어<br>
반복되는 동일한 함수호출을 줄인다. 이때 함수결과를 저장할 수 있는 공간이 Memoization table이다.

---
layout: post
title: Data Structure, Complexity of priority queue and heap sort
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Complexity of priority queue and heap sort

<center>
<figure>
<img src="/assets/post-img/DataStructure/45.png" alt="" width="50%">
</figure>
</center>

### Heap Sort

- 최대 힙 트리나 최소 힙 트리를 구성해 정렬을 하는 방법
- 내림차순 정렬을 위해서는 최대 힙을 구성하고 오름차순 정렬을 위해서는 최소 힙을 구성
  - 정렬해야 할 n개의 요소들로 최대 힙(완전 이진 트리 형태)을 만든다
  - 내림차순을 기준으로 정렬
  - 그 다음으로 한 번에 하나씩 요소를 힙에서 꺼내서 배열의 뒤부터 저장
  - 삭제되는 요소들(최댓값부터 삭제)은 값이 감소되는 순서로 정렬

### Priority queue

- sorting 해야할 값을 가지고 있음
- 이를 insert함 > O(NlogN)

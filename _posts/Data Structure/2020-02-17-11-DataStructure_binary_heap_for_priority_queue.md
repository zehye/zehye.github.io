---
layout: post
title: Data Structure, Binary heap for priority queue
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Binary heap for priority queue

- linked list based implementation
  - sorted implementation
  - Unsorted implementation
- tree based implementation
  - Tree should be balanced justify the reason of using trees
  - 즉 밸런스가 보장되어야지만 실제 사용에 있어 효용이 나게 된다


### Binary heap = Binary tree

<center>
<figure>
<img src="/assets/post-img/DataStructure/40.png" alt="" width="50%">
</figure>
</center>

Binary tree = 1 parent 2 childs

- shape property: complete tree 모양이 되어야 한다  
- heap property: 부모 노드가 자식 노드보다 value 가 더 커야 한다
- 위 이미지는 full tree는 아니지만 complete tree는 맞다


<center>
<figure>
<img src="/assets/post-img/DataStructure/41.png" alt="" width="50%">
</figure>
</center>

- 위 이미지는 complete tree 자체가 아니다


### Structure of binary heap using reference

<center>
<figure>
<img src="/assets/post-img/DataStructure/42.png" alt="" width="50%">
</figure>
</center>

Binary heap 은 array 로도 구현이 가능하지만 reference structure로도 구현 가능하다.

<center>
<figure>
<img src="/assets/post-img/DataStructure/43.png" alt="" width="50%">
</figure>
</center>



#### Insert operation of binary heap

- Percolate-up
  - 우리나라 용어로는 '삼투현상', 뿌리에서부터 나무로 올라간다는 의미
  - 즉 자식 노드부터 root 로 올라간다는 의미

- 어떻게 구현할까?
  - 삽입하려는 value 들을 다음 노드에 넣어준다.
    - 어떻게 다음 노드가 무엇인지 아는가?
    - structed property -> complete tree 조건을 지켜야하기 때문에
    - heap property : **부모 노드 > 자식 노드** -> 이 조건이 안맞는 경우 percolate-up 을 실행
  - 부모노드보다 새로 삽입되는 노드가 큰지 비교
    - heap property is broken 한 상황이고
    - exchange the two values 하게 된다

```python

```

#### Delete operation of binary heap

- Percolate-down
  - 제일 가장 큰 value 는 root
  - root 가 없어진다는 가정을 하는 경우 complete 구조가 깨짐
  - 가장 최근의 노드 value를 노드 value로 올림 → 이런경우 heap property 를 만족시키지 못함

- 어떻게 구현할까?
  1. root 노드 값
  2. last node value 를 복사
  3. last node value 와 childer node 비교
      - 어떤거와 비교? childer node 중에서 가장 큰 노드로 변경

```python

```

- rigth 가 없는 경우 left가 존재해야한다 (complete 구조이기 때문)

---
layout: post
title: Data Structure, Priority Queue  
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Priority Queue

1. Priority와 함께 Enqueue > 우선적으로 Dequeue가 되도록 한다.
2. Higher value = Higher priority, Lower value = Lower priority
3. 기본적으로 Enqueue이지만, priority value가 포함되어 있다 (element, key)


### How to implement priority queues

1. linked list를 기본적으로 사용한다.
2. priority와 함께 요소를 저장한다.

- Lazy approach == Unsorted implementation
  - Enqueue(처음 저장될때에는) priority별로 sorting이 안되어있지만 Dequeue할때 급히 sorting된다
- Early-bird approach == Sorted implementation
  - 처음부터 자기 자리를 찾아서 Enqueue한다.


### Implementation of priority queues

```python
class PriorityNode:
  priority = -1
  value = ''

  def __init__(self, value, priority):
    self.priority = priority
    self.value = value

  def get_value(self):
    return self.value

  def get_priority(self):
    return self.priority

class PriorityQueue:
  list = ''

  def __init__(self):
    self.list = SingliLinkedList()

    def enqueue_with_priority(self, value, priority):
      index_insert = 0
      for itr in range(self.list.get_size()):
        node = self.list.get(itr)
        if node.get_value() == '':  
          index_insert = itr
          break  # tail을 heap
        if node.get_value().get_priority() < priority:
          index_insert = itr
          break  #
        else:
          index_insert = itr + 1
      self.list.insert_at(PriorityNode(value, priority), index_insert)

    def dequeue_with_priority(self):
      return self.list.remove_at(0).get_value()
```


<center>
<figure>
<img src="/assets/post-img/DataStructure/38.jpeg" alt="" width="50%">
</figure>
</center>

#### Performances of priority queue implementations

<center>
<figure>
<img src="/assets/post-img/DataStructure/39.png" alt="" width="50%">
</figure>
</center>

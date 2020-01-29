---
layout: post
title: Data Structure, Tree Traversing
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Tree Traversing

- 트리는 linked list보다 복잡하다
- multiple way가 가능하다 > 다양한 선택지가 가능하다.


### Depth first traverse

LHS를 먼저팔지, RHS를 먼저팔지를 recursion으로 처리하는 traversing 기법<br>
-> LHS가 더 작은 값이기 때문에 일반적으로 먼저 프린트한다. 그럼 이때 이 LHS와 RHS를 가리키고 있는 current value는 어디에 프린트해야할까?

1. Pre-order traverse : traversing이 일어나기전에 출력 > 3 2 0 1 5 4 7 6 9 8
2. In-order traverse : LHS와 RHS사이에 출력 > 0 1 2 34 5 6 7 8 9
3. Post-order traverse : traversing이 일어난 후에 출력 > 1 0 2 4 6 8 9 7 5 3


```python
def traversePreOrder(self, node=''):
  if node == '':
    node = self.root
  result = []
  result.append(node.getValue())
  if node.getLHS() != '':
    result = result + self.traversePreOrder(node.getLHS())
  if node.getRHS() != '':
    result = result + self.traversePreOrder(node.getRHS())
  return result

def traverseInOrder(self, node=''):
  if node == '':
    node = self.root
  result = []
  if node.getLHS() != '':
    result = result + self.traverseInOrder(node.getLHS())
  result.append(node.getValue())
  if node.getRHS() != '':
    result = result + self.traverseInOrder(node.getRHS())
  return result

def traversePostOrder(self, node=''):
  if node == '':
    node = self.root
  result = []
  if node.getLHS() != '':
    result = result + traversePostOrder(node.getLHS())
  if node.getRHS() != '':
    result = result + traversePostOrder(node.getRHS())
  result.append(node.getValue())
  return result
```


### Breath first traverse

Recursion이 아닌 Queue를 통한 traverse > Level order traversing

- root를 enqueue한다
- queue가 비어질때까지 계속 while문을 돈다
- 첫번째 요소는 dequeue > 첫번째 current value를 출력
- root의 LHS가 존재한다면 queue에 enqueue하고 다음에 RHS가 존재한다면 enqueue한다 > 그러고 while문을 또 돈다

<center>
<figure>
<img src="/assets/post-img/DataStructure/32.png" alt="" width="50%">
</figure>
</center>

즉 level별로 traversing한다.


```python
def traverselevelOrder(self):
  result = []
  queue = Queue()
  queue.enqueue(self.root)
  while queue.isEmpty == False:
    node = queue.dequeue()
    if node == '':
      continue
    result.append(node.getValue())
    if node.getLHS() != '':
      queue.enqueue(node.getLHS())
    if node.getRHS() != '':
      queue.enqueue(node.getRHS())
      queue.enqueue(node.getRHS())
  return result
```


## Performance of binary search tree

binary search tree의 경우 height의 갯수만큼 데이터를 찾기만 하면 된다.

<center>
<figure>
<img src="/assets/post-img/DataStructure/33.png" alt="" width="50%">
</figure>
</center>

두 그림 모두 트리가 맞지만 두개의 성능 차이는 존재한다.

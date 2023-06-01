---
layout: post
title: Data Structure, Binary Search Tree
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Binary Search Tree(BST): a simple structure

degree가 2인 트리 > 그중에서도 search에 특화되어 있는 트리

- 저장된 데이터의 빠른 탐색을 위해 디자인된 트리
- 규칙을 데이터구조에 저장해놓음으로써 시간을 단축해 데이터를 찾아낼 수 있다.


### 예시

<center>
<figure>
<img src="/assets/post-img/DataStructure/28.png" alt="" width="50%">
</figure>
</center>

위와 같은 데이터가 있다고 가정한 상태에서 아래 linked list와 binary search tree를 비교해 보자

<center>
<figure>
<img src="/assets/post-img/DataStructure/29.png" alt="" width="50%">
</figure>
</center>

linked lsit의 경우 순서대로 한줄로 나열되어질 것이다. 그러나 binary search tree의 경우에는 규칙이 정해져있다고 생각해보자.<br>
1003이라는 root에서 왼쪽 next로 빠지는 노드들은 1003보다 작은 계좌이고 오른쪽 next로 빠지는 노드는 1003보다 큰 숫자의 계좌번호라는 규칙을 세워놓는 것이다. 이렇게 규칙을 만들어놓음으로써 즉, 규칙을 데이터 구조에 저장해놓음으로써 원하는 데이터를 얼마의 시간을 들이지 않고 찾아낼 수 있도록 할 수 있다.


### Implementation of tree node

1. 4개의 레퍼런스 노드를 가진 트리노드를 만들어준다.
2. 왼쪽은 작은값을 오른쪽은 큰값을 가지는 노드를 가리키도록 한다.
3. 데이터들을 가져오고 사용할수 있는 get, set 함수를 정의해준다.

```python
class TreeNode:
  nodeLHS = ''
  nodeRHS = ''
  nodeParent = ''
  value = ''

  def __init__(self, value, nodeParent):
    self.value = value
    self.nodeParent = nodeParent

  def getLHS(self):
    return self.nodeLHS
  def getRHS(self):
    return self.nodeRHS
  def getValue(self):
    return self.value
  def getParent(self):
    return self.nodeParent
  def setLHS(self, nodeLHS):
    self.nodeLHS = nodeLHS
  def setRHS(self, nodeRHS):
    self.nodeRHS = nodeRHS
  def setValue(self, value):
    self.value = value
  def setParent(self, nodeParent):
    self.nodeParent = nodeParent
```


### Binary Search Tree Implementation

- 루트에 대한 레퍼런스만 저장해놓는다.

```python
class BinarySearchNode:
  root = ''
```



#### Insert operation of binary search tree

```python
def insert(self, value, node=''):
  if node == '':
    node = self.node
  if self.root == '':
    self.root = TreeNode(value, '')
    return
  if value == node.getValue():
    return

  if value > node.getValue():
    if node.getRHS() == '':
      node.setRHS(TreeNode(value, node))
    else:
      self.insert(value, node.getRHS())

  if value < node.getValue():
    if node.getLHS() == '':
      node.setLHS(TreeNode(value, node))
    else:
      self.insert(value, node.setLHS())
  return
```


<center>
<figure>
<img src="/assets/post-img/DataStructure/30.png" alt="" width="50%">
</figure>
</center>


#### Search operation of binary search tree


```python
def search(self, value, node=''):
  if node == '':
    node = self.root
  if value == node.getValue():
    return True

  if value > node.getValue():
    if node.getRHS() == '':
      return False
    else:
      return self.search(value, node.getRHS())

  if value < node.getValue():
    if node.getLHS() == '':
      return False
    else:
      return self.search(value, node.getLHS())
```

즉, search를 하기위해 데이터 n개를 모두 볼 필요없이 해당하는 height(Maximum Depth)만큼만 보면 된다.<br>
규칙에 따라 트리를 만들어놨기 때문에 linked list보다 binary search tree가 성능이 더 좋은것!


#### Delete operation of binary search tree

트리 구조하에서 노드를 삭제하려한다면 각 노드마다 조건이 다 다르다.

- 1. no children: 지우고자 하는 노드의 부모에 가서 레퍼런스를 끊어준다
- 2. one child: 지우고자 하는 노드의 부모 레퍼런스가 마지막 노드를 가리키게 한다
- 3. two child: 다른 노드를 해당 노드에 넣는다 > LHS의 maximum값 or RHS의 minimun값
  - LHS의 maximum값: RHS를 계속 타고 내려감 > 오리지널 값을 지움 > one children
  - RHS의 minimun값: LHS를 계속 타고 내려감 > 오리지널 값을 지움 > no children

```python
def delete(self, value, node=None):
  if node is None:
    node = self.root
  if node.getValue() < value:
    return self.delete(value, node.getRHS())
  if node.getValue() > value:
    return self.delete(value, node.getLHS())

  if node.getValue() == value:
    if node.getLHS() is not None and node.getRHS() is not None:
      nodeMin = self.findMin(node.getRHS())
      node.setValue(nodeMin.getValue())
      self.delete(nodeMin.getValue(), node.getRHS())
      return
    parent = node.getParent()
    if node.getLHS() is not None:
      if node == self.root:
        self.root = node.getLHS()
      elif parent.getLHS() == node:
        parent.setLHS(node.getLHS())
        node.getLHS().setParent(parent)
      else:
        parent.setRHS(node.getLHS())
        node.getLHS().setParent(parent)
```


#### Minimum and Maximum in binary search tree

<center>
<figure>
<img src="/assets/post-img/DataStructure/31.png" alt="" width="50%">
</figure>
</center>


```python
def findMax(self, node=''):
  if node == '':
    node = self.root
  if node.getRHS():
    return node
  return self.findMax(node.getRHS())

def findMin(self, node=''):
  if node == '':
    node = self.root
  if node.getLHS():
    return node
  return self.findMin(node.getLHS())  
```

---
layout: post
title: Data Structure, Linked list
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>


## Reference structure

linked list는 index 구조가 아닌 reference 구조로 중간의 공간을 만들어줄 수 있는 자료구조이다.<br>
reference 구조: 위치를 가리키는 것

예시를 통해 레퍼런스 구조에 대해 이해해보자.

```python
x = [1,2,3]  # [1,2,3]
y = [100,x,120]  # [100,[1,2,3],120]
z = [x,'a','b']  # [[1,2,3],'a','b']

x[1]=1717

print(x)  # [1,1717,3]
print(y)  # [100,[1,1717,3],120]
print(z)  # [[1,1717,3],'a','b']

x[1]=2
x2=[1,2,3]

if x == x2:
  print("값이 동일하다")
else:
  print("값이 동일하지 않다")

if x is x2:
  print("값이 같은 곳에 저장되어 있다")
else:
  print("값이 다른곳에 저장되어 있다")

if x[1] is y[1][1]:
  print("값이 같은 곳에 저장되어 있다")
else:
  print("값이 다른곳에 저장되어 있다")

# 값이 동일하다
# 값이 다른곳에 저장되어 있다
# 값이 같은 곳에 저장되어 있다
```

저장된 곳의 값을 바꾸면 저장된 곳을 가리키고 있는 많은 다른 변수들도 변화된 값을 가지게 된다.



## Basic Structure: Singly linked list

노드와 레퍼런스로 이루어져 있다.

`노드(node)`: 두개의 변수로 이루어져있으며 이는 클래스로 구현하는게 좋다.

첫번째 변수는 **next node** 를 가리키고 있다.(이 노드의 다음 노드가 무엇인가를 저장, 레퍼런스 구조를 저장한다)<br>
두번째 변수는 **value object**, 즉 어떤 값이 저장될 것인지 그 밸류를 저장하고 있는 레퍼런스를 저장

`special node`: head와 tail이 있다 > Singly linked list 에서 꼭 저장할 필요는 없지만 해두면 유용하다.

**head**: list의 맨 처음 노드에 저장 > 아무값도 저장되어있지 않고 next는 존재. 그러나 head앞의 next는 존재하지 않는다.<br>
**tail**: 맨 마지막에 있는 노드 > 아무값도 저장되어있지않고 next도 없는 리스트의 마지막 노드

<center>
<figure>
<img src="/assets/post-img/DataStructure/3.jpeg" alt="" width="80%">
</figure>
</center>

중간에 있는 레퍼런스 구조를 조작하여 공간을 한번에 만들어낼 수 있는 장점이 있다.



### Implementation of Node Class

노드는 어떻게 만들 수 있는가?


```python
class Node:
  nodeNext = ''
  objValue = ''
  blnHead = False
  blnTail = False


  def __init__(self, nodeNext='', objValue='', blnHead=False, blnTail=False):
    self.nodeNext = nodeNext
    self.objValue = objValue
    self.blnHead = blnHead
    self.blnTail = blnTail

  def getValue(self):
    return self.objValue

  def setValue(self, objValue):
    self.objValue = objValue

  def getNext(self):
    return self.nodeNext

  def setNext(self, nodeNext):
    self.nodeNext = nodeNext

  def isHead(self):
    return self.blnHead

  def isTail(self):
    return self.blnTail
```

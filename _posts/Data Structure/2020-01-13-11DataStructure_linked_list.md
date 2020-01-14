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

`specialized node`: head와 tail은 specialized node라고 한다. 이유는 Head와 Tail의 위치가 정해져있기 때문이다.<br>
 -> Singly linked list 에서 꼭 저장할 필요는 없지만 해두면 유용하다.

**head**: list의 맨 처음에 있는 노드 > 아무값도 저장되어있지 않고 next는 존재. 그러나 head앞의 next는 존재하지 않는다.<br>
**tail**: list의 맨 마지막에 있는 노드 > 아무값도 저장되어있지 않고 next도 없는 리스트의 마지막 노드


#### Empty Linked List

<center>
<figure>
<img src="/assets/post-img/DataStructure/4.jpeg" alt="" width="80%">
</figure>
</center>

start와 end를 가리키는 linked list를 구성하는 핵심적인 요소가 된다.

<center>
<figure>
<img src="/assets/post-img/DataStructure/3.jpeg" alt="" width="80%">
</figure>
</center>

중간에 있는 레퍼런스 구조를 조작하여 공간을 한번에 만들어낼 수 있는 장점이 있다.

사실 linked list에서는 head와 tail이 존재하지 않더라도 구현은 가능하다. 그러나 있으면 훨씬 편하게 구현이 가능하다.

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


### Search procedure in Singly linked list

1. 해당 리스트로부터 d와 c를 찾는다
2. array와 마찬가지로 처음부터 끝까지 순서대로 확인한다.
3. 패턴(차례대로 순회) 차원에서는 달라지는것은 없지만, 인덱스는 사용할 수 없다. (next를 사용)

<center>
<figure>
<img src="/assets/post-img/DataStructure/5.jpeg" alt="" width="80%">
</figure>
</center>

1) 맨 처음 해야할일은 list로 부터 head를 찾는다. <br>
2) head의 next node를 찾는다. <br>
3) next가 tail인지 아닌지를 확인한다.

```python
if next == tail:
  break
if next != tail:
  next.objValue == 'd'
  # next.next
```

N번의 operation을 통해 유무를 찾아볼 수 있다.


### Insert procedure in Singly linked list > b와 d사이에 c를 넣는다

linked list에서 자료를 넣는 방법 > power of a linked list

크게 3개의 operation이 필요하다!

<center>
<figure>
<img src="/assets/post-img/DataStructure/6.jpeg" alt="" width="80%">
</figure>
</center>

1. 어디에 넣고싶은지는 알고있어야 한다. > `node prev`, `node next`
2. node new에서의 next는 무엇인지 모르는 상황이다.
3. node prev와 node next 사이의 연결을 끊는다.
4. 이때 node prev의 next값을 node new로 향하게 한다. (nodeprev.next = nodenew)
5. node new의 next는 node next로 연결되도록 한다. (nodenew.next = nodenext)



### Delete procedure in Singly linked list > d를 삭제한다

linked list에서 자료를 삭제하는 방법 > power of a linked list

크게 3개의 operation이 필요하다!

<center>
<figure>
<img src="/assets/post-img/DataStructure/7.jpeg" alt="" width="80%">
</figure>
</center>

1. 무엇을 삭제할 것인지는 알고있다. > `node prev`, `node remove`, `node next`
2. 현재 상황 : node remove = nodeprev.next, node next = nodeprev.next.next
3. node prev에서 node remmove로 가는 길을 끊어준다.
4. node prev의 next를 node next를 향하게 한다. (nodeprev.next = nodenext)


#### 그렇게 된다면 `d`는 어디에 있을까?

GC를 통해 `d`에 대한 메모리를 삭제시켜준다. 궁극적으로 우리가 삭제시키는 것은 아니다.



### Implementation of Singly linked list

```python
class SinglyLinkedList:
  nodeHead = ''
  nodeTail = ''
  size = 0

  def __init__(self):
    self.nodeTail = Node(blnTail=True)
    self.NodeHead = Node(blnHead=True, nodeNext=self.nodeTail)

  def insert(self, objInsert, idxInsert):
    nodeNew = Node(objValue = objInsert)
    nodePrev =  self.get(idxInsert -1)
    nodeNext = nodePrev.getNext()
    nodePrev.setNext(nodeNew)
    nodeNew.setNext(nodeNext)
    self.size = self.size +1

  def removeAt(self. idxRemove):
    nodePrev = self.get(idxRemove-1)
    nodeRemove = nodePrev.getNext()
    nodeNext = nodeRemove.getNext()
    nodePrev.setNext(nodenext)
    self.size = self.size -1
    return nodeRemove.getValue()

  def get(self.idxRetrieve):
    nodeReturn = self.nodeHead
    for itr in range(idxRetrieve +1):
      nodeReturn = nodeReturn.getNext()
    return nodeReturn

  def printStatue(self):
    nodeCurrent = self.nodeHead
    while nodeCurrent.getNext().isTail() == False:
      nodeCurrent = nodeCurrent.getNext()
      print(nodeCurrent/getValue(), )
    print

  def getSize(self):
    return self.size
```

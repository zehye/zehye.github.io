---
layout: post
title: Data Structure, Stack and Queue
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>


## Stack and Queue

linked list를 활용해 구현할 수 있는 자료구조


## Stack

데이터를 넣고 뺄 수 있는 곳이 한곳인 것



### Structure of Stack

linked list와 같이 선형으로 이루어져있다. > A variable of a singliy linked list

다른점: linked list처럼 중간에 데이터를 넣고 빼는 행위를 하지 않는다.

우리가 의도적으로 stack이 정의가 되면 linked list의 중간에 있는 것은 관여를 하지 않는다.<br>
오로지 첫번째 노드에 대해서만 관여를 한다.(이에 대해서만 데이터를 넣고 뺀다)

이 첫번째 노드를 `top`이라고 한다. 리스트의 첫번째 인스턴스 = top

insert, remove는 오로지 top을 통해서만 이루어지고 이 메카니즘을 **Last-In-First-Out(LIFO)메카니즘** 이라고 한다.

<center>
<figure>
<img src="/assets/post-img/DataStructure/8.jpeg" alt="" width="80%">
</figure>
</center>

- Cargo object1: 맨 마지막으로 들어가서 맨 처음으로 나올 object
- Cargo object6: 맨 처음으로 들어가서 맨 마지막으로 나올 object



### Operation of Stack

top에 한해서 insert, delete가 가능하다

<center>
<figure>
<img src="/assets/post-img/DataStructure/9.jpeg" alt="" width="80%">
</figure>
</center>

- push(=insert과 같은 역할): linked list의 첫번째 인스턴스를 insert한다
- pop(=delete과 같은 역할): linked list의 첫번째 인스턴스를 remove한다

즉, stack속에 linked list를 넣어 push, pop을 진행한다.



### Implementation of Stack

```python
class Stack:
  firstInstance = SinglyLinkedList()

  def pop(self):
    return self.firstInstance.removeAt(0)

  def push(self):
    return self.firstInstance.insertAt(value, 0)

stack = Stack()
stack.push('a')
stack.push('b')
stack.push('c')

print(stack.pop())  # c
print(stack.pop())  # b
print(stack.pop())  # a
```



#### Algorithm for the balanced  symbol checking

`7+{[2+(1+2)]*3}`

1. 빈 스택을 하나 만들어준다
2. 연산식의 끝까지 symbol들을 읽어내려간다.
- 만약 symbol이 열려지는 symbol이라면 push를 한다.
- 만약 symbol이 닫혀지는 symbol인데 스택이 비어있다면 에러다.
- 그렇지 않으면 pop을 하는데, pop의 symbol의 스타일이 매칭이되지 않으면 에러다.
3. 모든 연산이 끝나는 때에 스택이 비어있고 에러가 뜨지 않는다면 성공!

<center>
<figure>
<img src="/assets/post-img/DataStructure/10.jpeg" alt="" width="80%">
</figure>
</center>


## Queue

데이터가 들어가는 곳과 나가는 곳이 다른 것 (중간에 들어갈 수 있는 곳도 없다)


### Structure of Queue

linked list와 같이 선형으로 이루어져있다. > A variable of a singliy linked list

다른점: 중간 단계의 접근은 불가능하다. 오로지 들어오는 길은 맨 마지막과 맨처음이 된다.

맨처음으로 나갈 수 있고, 맨 마지막으로 들어올 수 있다. 리스트의 첫번째 인스턴스는 나올 수 있고 마지막 인스턴스를 통해 들어올 수 있다.

<center>
<figure>
<img src="/assets/post-img/DataStructure/11.jpeg" alt="" width="80%">
</figure>
</center>

이 메카니즘을 **Fast-In-First-Out(FIFO)메카니즘** 이라고 부른다.


### Operation of Queue

queue에도 리스트의 특정 인덱스에대해 처리만 해주면 된다.

- Enqueue: linked list의 마지막 위치에 요소를 insert해주는 것
- Dequeue: linked list의 첫번째 요소를 remove해주는 것

<center>
<figure>
<img src="/assets/post-img/DataStructure/12.jpeg" alt="" width="50%">
</figure>
</center>


### Implementation of Queue

```python
class Queue:
  firstInstance = SinglyLinkedList()

  def dequeue(self):
    return self.firstInstance.removeAt(0)

  def enqueue(self):
    return self.firstInstance.insertAt(value, self.firstInstance.getSize())

queue = Queue()
queue.enqueue()
queue.enqueue()
queue.enqueue()

print(queue.dequeue())  # a
print(queue.dequeue())  # b
print(queue.dequeue())  # c
```

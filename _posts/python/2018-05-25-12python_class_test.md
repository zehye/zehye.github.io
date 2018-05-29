---
layout: post
title: python class 연습문제 풀이
category: python
tags: [python, class]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 `python` 클래스 실습문제를 다시 정리해봅니다.

<hr>

1.일상생활에서 접할 수 있는 서로 연관되는 어떠한 것들에 대하여 3개의 클래스를 만들고, 각각에게 영향을 줄 수 있는 메서드를 만들고 사용해서 다른 인스턴스의 속성에 영향을 주는 코드를 작성해본다.

ex) 사람 클래스와 고양이 클래스 -> 사람이 '먹이준다' 메서드 실행 시 고양이 인스턴스를 전달, 고양이 인스턴스의 포만감을 +


```python
class Human:

  def __init__(self, name):
    self.name = name

  def give_prey(self, person):
    self.person = person
    print(f'{self.name}이 먹이를 줍니다.')

class Cat(Human):

  def __init__(self, name, item):
    super().__init__(name)

```

2.외부에서 조작하면 문제가 생길 수 있는 속성을 `private`하게 지정되도록 이름을 바꾸고, `property`를 만들어 본다. `setter`가 필요없는 속성은 읽기전용으로 남겨두며, 변경해야 하는 속성은 `setter`를 구현하고 제한조건을 만든다.

ex) 음식을 먹고 포만감을 늘리는 메서드 -> 포만감이 100이상이면 먹지 않도록

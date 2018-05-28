---
layout: post
title:  python class 문제풀이
categories: [python]
---
이 포스팅에서는 `python`의 `class`실습 문제 풀이를 하겠다.
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

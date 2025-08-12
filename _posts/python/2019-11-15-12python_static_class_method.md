---
layout: post
title: staticmethod와 classmethod 한번 더 정리해보기
category: Python
tags: [python, staticmethod, classmethod]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>


클래스의 인스턴스를 통하지 않고 클래스에서 바로 메서드를 호출하는 방법에는 정적 매서드와 클래스 메서드가 있다.


## Staticmethod

우선 정적 메서드는 메서드 위에 `@staticmethod`을 붙이며 매개변수로 `self`를 지정하지 않는다.


```
class 클래스이름:
  @staticmethod
  def 메서드(매개변수1, 매개변수2)
    코드
```

```python
class Calc:

  @staticmethod
  def add(a,b):
    print(a+b)

  @staticmethod
  def multi(a,b):
    print(a*b)

Calc.add(10,20) # 30
Calc.multi(3,5) # 15
```

Calc 클래스에서 `@staticmethod` 를 붙여서 add와 multi 메서드를 만들었고, **정적 메서드를 호출할 때는 클래스에서 바로 메서드를 호출하면 된다.** 정적 메서드는 self 를 매개변수로 받지 않으며 그렇기 때문에 인스턴스 속성에도 접근이 불가능하다. 그래서 보통 정적메서드는 인스턴스 속성, 인스턴스 메서드가 필요 없을 때 사용한다.

즉, 여기서 만든 Calc 클래스에 들어가있는 add, multi 메서드는 숫자 두개를 더하거나 곱할 뿐 인스턴스의 속성은 필요하지 않음을 의미한다. 이러한 정적메서드는 메서드의 실행이 외부 상태에 영향을 끼치지 않는 순수함수(pure function)을 만들때 사용한다. 따라서 저적 메서드는 인스턴스의 상태를 변화시키지 않는 메서드를 만들때 사용한다.



## Classmethod

클래스메서드 또한 메서드 위에 `@classmethod` 를 붙이고 첫번째 매개변수로 `cls`를 지정한다.

```
class 클래스이름:  
  @classmethod
  def 매서드(cls, 매개변수1, 매개변수2)
    코드
```

```python
class Person:
  count = 0

  def __init__(self):
    Person.count += 1 # 인스턴스가 만들어질때 클래스 속성 count에 1을 더함


  @classmethod
  def print_count(cls):
    print(f'{cls.count}명 생성 되었습니다.') # cls로 클래스 속성에 접근


jane = Person()
mike = Person()

Person.print_count() # 2명 생성되었습니다.
```

코드를 풀이해보면

- 인스턴스가 만들어질때마다 숫제를 세어주는 `count += 1` 을 만든다.
- 이때 클래스 속성에 접근하는 것을 명확히 하기 위해 `Person.count`로 만들어준다.
- 그리고 `@classmethod`를 통해 클래스 메서드를 생성한다.
- 이때 클래스 메서드는 첫번째 매개변수를 `cls`로 받음으로써 현재 클래스를 받는다.
- 따라서 cls를 통해 count 속성에 접근할 수 있게 된다.

- 우리는 아래서 jane, mike 두개의 Person 인스턴스를 만들었고
- print_count 호출하게 되면 생성된 두개의 인스턴스를 출력해주는 것을 볼 수 있다.
- print_count는 클래스 메서드이기때문에 `Person.print_count()`로 호출된다.


클래스 메서드는 정적 메서드처럼 인스턴스 없이 호출된다는 점은 같다. 그러나

- 클래스 메서드는 메서드 안에서 클래스 속성, 클래스 메서드에 접근해야할 때 사용한다.

특히 cls를 사용하면 메서드 안에서 현재 클래스의 인스턴스를 만들 수도 있다. cls는 클래스이기때문에 **cls()는 Person()과 같다**

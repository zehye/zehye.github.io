---
layout: post
title: 클래스 속성과 인스턴스 속성 알아보기
category: Python
tags: [python, class]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

우선적으로 이야기 하자면
- 클래스 속성 : 모든 인스턴스가 공유. 인스턴스 자체가 사용해야 하는 값을 저장할 때 사용
- 인스턴스 속성: 인스턴스 별로 독립되어 있음. 각 인스턴스가 값을 따로 저장해야 할 때 사용

## 클래스 속성 사용하기

```
class 클래스이름:
  속성 = 값
```

```python
class Person:
  bag = []


  def put_bag(self, stuff):
    self.bag.append(stuff)


jane = Person()
jane.put_bag('책')

mike = Person()
mike.put_bag('안경')

print(jane.bag)
print(mike.bag)
```

```
['책', '열쇠']
['책', '열쇠']
```

우리는 jane과 mike 각각의 인스턴스를 만들고자 put_bag 메서드에 물건을 넣었는데, 출력을 해보면 넣었던 물건들이 합쳐져서 나오는 것을 볼 수 있다. **즉, 클래스 속성은 클래스에 속해 있으며 모든 인스턴스에서 공유하고 있다.**

그래서 우리가 put_bag 메서드에서 클래스 속성 bag 에 접근하기 위해 self를 사용했는데, 사실 self는 현재의 인스턴스를 뜻하는 것이기 때문에 클래스 속성을 지칭하기에는 조금 모호한 표현이다. 그래서 아래와 같이 클래스 속성에 접근할 때는 클래스 이름으로 접근하면 좀 더 코드가 명확해진다.

```python
def put_bag(self, stuff):
  Person.bag.append(stuff)


print(Person.bag)
```

일반적으로 파이썬에서는 속성과 메서드 이름을 찾을 때 `인스턴스 > 클래스` 순으로 찾는다. 그래서 인스턴스 속성이 없으면 클래스 속성을 찾게 되므로 `jane.bag`혹은 `mike.bag`을 해도 문제없이 동작은 한다. 겉보기에는 인스턴스 속성을 사용하는 것 같지만 실제로는 클래스 속성이다.



## 인스턴스 속성 사용하기

```python
class Person:
  def __init__(self):
    self.bag = []

  def put_bag(self, stuff):
    self.bag.appennd(stuff)


jane = Person()
jane.put_bag('책')

mike = Person()
mike.put_bag('안경')

print(jane.bag)
print(mike.bag)
```

```
['책']
['열쇠']
```

즉, 인스턴스 속성은 인스턴스별로 독립되어 있어 서로 영향을 주지 않는 것으로 볼 수 있다.



### 비공개 클래스 속성 사용하기

클래스 속성도 비공개 속성을 만들 수 있는데, 그 방법은 클래스 속성을 만들때 아래와 같이 만들면 된다. 이 속성은 클래스 안에서만 접근할 수 있고 클래스 바깥에서는 접근이 불가능하다.

```
class 클래스이름:  
  __속성 = 값
```

즉, 클래스에서 공개하고 싶지 않은 속성이 있다면 비공개 속성을 사용하면 된다.

```python
class Knight:
  __item_limit = 10 # 비공개 클래스 속성

  def print_item_limit(self):
    print(Knight.__item_limit)  # 클래스 안에서는 접근 가능


x = Knight()
x.print_item_limit()

print(Knight.__item_limit) # 클래스 바깥에서는 접근 불가능
```

```
10
Traceback (most recent call last):
  File "C:\project\class_private_class_attribute_error.py ", line 11, in <module>
    print(Knight.__item_limit)
AttributeError: type object 'Knight' has no attribute '__item_limit'
```

실행을 하면 클래스 Knight의 비공개 클래스 속성 `__item_limit`는 클래스 안의 print_item_limit 메서드에만 접근할 수 있고 클래스 바깥에서 접근하면 에러가 발생한다. 아이켐의 보유 제한은 10개인데, 이 클래스를 사용하는 사람이 마음대로 수정하면 곤란하니 그럴때 사용하면 된다. **즉, 비공개 클래스 속성은 클래스 바깥으로 드러내고 싶지 않은 값에 사용한다**

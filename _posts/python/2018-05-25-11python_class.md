---
layout: post
title: python class
category: python
tags: [python, class]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 `python` 클래스에 대해 설명합니다.

<hr>

## class

### 객체지향 프로그래밍

파이썬의 모든 것은 객체이며, 객체를 사용할 때는 변수에 해당 객체를 참조(`Reference`)시켜 사용한다.

객체는 변수와 함수를 가지며, 특별히 **객체** 가 가진 **변수** 와 **함수** 는 `속성(attribute)`과 `메서드(method)`라고 부른다.

이러한 객체는 어떠한 타입, 즉 특정한 **클래스** 의 형태를 가진 **인스턴스** 를 나타낸다.

```
즉, 클래스란 똑같은 무엇인가를 계속해서 만들어 낼 수 있는 설계 도면 같은 것이고,
객체란 클래스에 의해서 만들어진 피조물을 뜻한다.

이러한 클래스에 의해서 만들어진 객체에는 중요한 특징이 있는데, 그것은 객체별로 독립적인 성격을 갖는다는 것이다.  
(독립된 단위가 알아서 움직이도록 하는 것)

class Cookie:
  pass

a = Cookie()
b = Cookie()

라고 했을 때, Cookie()의 결과값을 받은 와 b는 객체이다.
그리고 일반적으로 객체와 인스턴스는 같은 맥락에서 사용한다.
```

### __init__ 초기화 메서드

객체를 생성할 때, 인자를 어떻게 전달받고, 받은 인자를 이용해 어떤 객체를 생성할 지 정의
내가 원하는 애가 무엇인지를 확인하기 위해서는 `self`가 필요하다.

```python
class Shop:
  def __init__(self, name):
    self.name = name
```

객체의 메서드를 정의할 때, 첫번째 인수는 항상 `self`이다. `self`에는 메서드를 호출하는 객체 자신이 자동으로 전달된다.

이렇게 `self`가 자동으로 호출되는 이유는 클래스의 메서드를 사용할 때 어떤 객체가 해당 메서드를 사용하고 있는 지 알 수 있도록 하기 위해서 이다. 또한 이렇게 하나의 메서드를 여러 객체가 공유할 수 있다.

속성이나 메서드에 접근할 때는 `객체.속성명`, `객체.메서드명`을 사용한다.

**문제1**

`Shop` 클래스에 `show_info`메서드를 정의하고 해당메서드는 `상점정보 (<상점명>)`을 출력한다.

```python
class Shop:
  def __init__(self, name):
    self.name = name

  def show_info(self):
    print('상점정보 ({})'.format(self.name))
```

### 클래스 속성

어떤 하나의 클래스로부터 생성된 객체들이 같은 값을 가지게 하고 싶은 경우, 클래스 속성(class attribute)를 사용한다.

```python
class Shop:
  description = 'Python Shop Class'
  def __init__(self, name):
    self.name = name

  def show_info(self):
    print('상점정보 ({})'.format(self.name))
```

마찬가지로, 객체들에게서 각각의 인스턴스와는 밸개의 공통된 메서드를 사용하게 하고 싶을 경우, 클래스 메서드(class method)를 사용한다.

### 메서드

**인스턴스 메서드**

인스턴스 메서드는 첫번째 인수로 `self`를 가진다. 인스턴스를 이용해 메서드를 호출할때, 호출한 인스턴스가 자동으로 전달되며, 전달받은 인스턴스는 상태를 확인하거나 조작하는 데에 사용된다.

**문제2**

`Shop`클래스의 초기화 메서드 인자로 `shop_type`과 `address`를 추가하고, 해당 인자들을 사용해 객체의 초기값을 만들어준다.

`Shop`클래스의 인스턴스 메서드 `show_info`를 아래와 같은 결과를 출력할 수 있도록 수정해본다.

```python
class Shop:
    def __init__(self, name, shop_type, address):
        self.name = name
        self.shop_type = shop_type
        self.address = address

    def show_info(self):
        print('상점정보 ({})'.format(self.name))
        print('유형: {}'.format(self.shop_type))
        print('주소: {}'.format(self.address))  
```

**문제3**

`Shop`클래스에 `change_type`인스턴스 메서드를 추가하고, 상점유형(shop_type)을 변경할 수 있는 기능을 추가한다.

새로운 `Shop`인스턴스를 하나 생성하고, `show_info()` 인스턴스 메서드를 사용해 본 후 `change_type`메서드를 사용해 `shop_type`을 변경시키고 다시 `show_info()`메서드를 실행해 결과가 잘 반영되었는지 확인한다.

```python
class Shop:
    def __init__(self, name, shop_type, address):
        self.name = name
        self.shop_type = shop_type
        self.address = address

    def show_info(self):
        print('상점정보 ({})'.format(self.name))
        print('유형: {}'.format(self.shop_type))
        print('주소: {}'.format(self.address))  

    def change_type(self new_shop_type,):
        self.shop_type = new_shop_type
```


### 속성 접근 지정자

**캡슐화**

객체를 구현할 때, 사용자가 반드시 알아야할 데이터나 메서드를 제외한 부분을 은닉시켜 정해진 방법을 통해서만 객체를 조작할 수 있도록 하는 방식

객체의 데이터나 메서드의 은닉 정도를 결정할 때, 속성 접근 지정자를 사용한다.

- `change_type`메서드나 `change_description`클래스 메서드를 사용하지 않고도 내부 내용을 변경 할 수 있다.

속성 이름을 `__`로 시작하면, 외부에서의 접근을 제한한다. 이 경우를 `private지정자`라고 한다.

파이썬은 속성을 실제로 사용하지 못하도록 숨기지 않고, 네임 맹글이라는 기법을 사용한다. 파이썬에서는 문법적으로 `private`데이터에 대한 접근을 막는 법을 제공하지 않는다. 이는 개발자에게 최대한 제약을 가하지 않는다는 파이썬의 철학때문이다.


### get/set속성값과 property

파이썬에서는 지원하지 않지만, 어떤 언어들은 외부에서 접근할 수 없는 `private`객체 속성을 지원한다. 이 경우, 객체에서는 해당 속성을 읽고 쓰기 위해 `getter`, `setter`메서드를 사용해야 한다.

파이썬에서는 해당 기능을 프로퍼티(`property`)를 사용해 간단히 구현한다.

```python
class Shop:
    def __init__(self, name, shop_type, address):
        self.name = name
        self.shop_type = shop_type
        self.address = address

    def show_info(self):
        print('상점정보 ({})'.format(self.name))
        print('유형: {}'.format(self.shop_type))
        print('주소: {}'.format(self.address))  

    def change_type(self, new_shop_type):
        self.shop_type = new_shop_type

    @property
    def shop_type(self):
        return self.__shop_type

    @shop_type.setter
    def shop_type(self, new_shop_type):
        self.__shop_type = new_shop_type
        print('Set new shop type ({})'.format(self.__shop_type))
```

`setter`프로퍼티를 명시하지 않으면, 읽기전용이 되어 외부에서 조작할 수 없게 된다.



### 상속

거의 비슷한 기능을 수행하나, 약간의 추가적인 기능이 필요한 다른 클래스가 필요할 경우 기존의 클래스를 상속받은 새 클래스를 사용하는 형태로 문제를 해결할 수 있다.

이때, 상속 되는 클래스를 부모(상위)클래스라고 하며, 상속을 받는 클래스는 자식(하위)클래스라고 한다.

상속을 받을때는 클래스의 정의 다음 괄오에 부모클래스를 적어주면 된다.

```python
class Restaurant(Shop):
  pass
```
상속받은 클래스는 부모 클래스의 모든 속성과 메서드를 사용할 수 있다.


**메서드 오버라이드**


상속받은 클래스에서, 부모클래스의 메서드와는 다른 동작을 하도록 할 수 있다. 이 경우 부모 클래스의 메서드를 덮어씌워서 사용하도록 하며, 이 방법을 메서드 오버라이드라고 한다.


**부모 클래스의 메서드 호출 (super)**

자식 클래스의 메서드에서 부모 클래스에서 사용하는 메서드의 전체를 새로 쓰는 것이 아닌, 부모 클래스의 메서드를 호출 후 해당 내용으로 새로운 작업을 해야할 경우 `super()`메서드를 사용해서 부모 클래스의 메서드를 직접 호출할 수 있다.

```python
class Restaurant(Shop):
  def __init__(self, name, shop_type, address, rating):
    super().__init(self, name, shop_type, address):
    self.rating = rating
```

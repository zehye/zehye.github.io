---
layout: post
title: python 함수 - args, kwargs, docstring
category: python
tags: [python, parameter, argument, docstring]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 `python` 함수에 대해 설명합니다.

<hr>

## 함수

매번 같은 코드를 계속 써야한다면 번거로우니, 반복적인 작업을 하는 코드를 재사용이 가능하게 정의해 놓은 함수를 만든다.

```python
def 함수명(매개변수[parameters]):
	동작
```

### 함수의 정의, 실행

```python
# 실행 시 'call func'를 print하는 함수 정의
>>> def func():
...   print('call func')
...

# 함수 자체는 function객체를 참조하는 변수
>>> func
<function func at 0x10dabf378>

# 함수를 실행시키기 위해 () 사용
>>> func()
call func

```

### 리턴값이 있는 함수 정의

```python
>>> def return_true():
...   return True
...
>>> return_true()
True
```

```python
def add(a,b):
    return a+b

add(5,7)
```


함수의 결과로 `bool`값을 갖는 데이터를 리턴하여 if 문이나 while문의 조건으로 사용가능하다.

### 매개변수를 사용하는 함수 정의

```python
>>> def print_arguments(something):
...   print(something)
...
>>> print_arguments('ABC')
ABC
```


### 매개 변수(parameter)와 인자(argument)의 차이

> 매개변수는 함수 내부에서 함수에게 전달되어 온 변수 즉, 함수를 정의할 때 들어오는 변수를 의미하고, 인자는 함수를 호출할 때 전달하는 변수 즉, 함수를 정의하고 나서 함수를 사용하는 변수를 의미한다.

```python
# 함수 정의때는 매개변수
def func(매개변수1, 매개변수2):
  ...

# 함수 호출시에는 인자
>>> func(인자1, 인자2)
```

*함수가 리턴값을 가지지 않는 경우는 `none`객체를 얻는다.*

### 위치 인자와 키워드 인자

**위치인자(positional arguments)**

매개변수의 순서대로 인자를 전달하여 사용하는 경우

```python
>>> def student(name, age, gender):
...   return {'name': name, 'age': age, 'gender': gender}
...
>>> student('hanyeong.lee', 31, 'male')
{'name': 'hanyeong.lee', 'age': 31, 'gender': 'male'}
```

**키워드 인자(keyword arguments)**

매개변수의 이름을 지정하여 인자로 전달하여 사용하는 경우

```python
>>> student(age=31, name='hanyeong.lee', gender='male')
{'name': 'hanyeong.lee', 'age': 31, 'gender': 'male'}
```

> 위치인자와 키워드 인자를 동시에 쓴다면, 위치인자가 먼저 와야 한다.

```python
def students(name, age, gender):
    return{
        'name':name,
        'age':age,
        'gender': gender
    }

students('jihye',gender='female',age=25)
```

### 기본 매개변수값 지정

인자가 제공되지 않을 경우, 기본 매개변수로 사용할 값을 지정할 수 있다.

```python
>>> def student(name, age, gender, cls='WPS'):
...   return {'name': name, 'age': age, 'gender': gender, 'class': cls}
...
>>> student('hanyeong.lee', 31, 'male')
{'name': 'hanyeong.lee', 'age': 31, 'gender': 'male', 'class': 'WPS'}
```

### 기본 매개변수값의 정의 시정

기본 매개변수 값은 함수가 실행될 때마다 계산되지 않고, 함수가 정의되는 시점에 계산되어 계속해서 사용된다.

```python
def return_list(value, result=[]):
  result.append(value)
  return result

# value라는 매개변수에 전달된 값을 새로운 리스트를 생성하는 result에 해당 값을 추가한 후 return해주는 함수

# 기존 리스트에 추가하는 형태로 사용

return_list('apple')

>>> ['apple']

return_list('banana')
>>> ['apple', 'banana']
# 값이 추가된 상태에서 다시 정의가 된다.
# 그렇기 때문에 위 ['apple']과 같은 메모리 상에 있다.
```

```python
id(return_list(123123))
id(return_list(456456))

>>>4572752008
```

따라서 함수가 실행되는 시점에 기본 매개변수 값을 계산하기 위해, 아래와 같이 바꿔준다.

```python
def return_list(value, result=None):
  if result is None:
    result = []
  result.append(value)
  return result

return_list('apple')
>>> ['apple']

return_list('banana')
>>> ['banana']
```


> 잠깐 =와 ==에 대해 이야기해보자면,

```python
list1 = []
list1=list2

list1 == list2
>>> True

////////////////

var1 = 123123123123
var2 = 123123123123

var1 == var2
>>> True

# 이때

id(var1)
>>> 4572156880

id(var2)
>>> 4572159120

var1 is var2
>>> False

a = 100
b = 100

a == b
>>> True

a is b
>>> True
# 이미 작은 숫자들은 파이썬 내부적으로 같게 나오도록 적용되어 있다.

a = 123123123123
b = 123123123123

a is b
>>> False

a = '123123123123'
b = '123123123123'

a is b
>>> True
```
어느정도의 최적화하느냐에 따라 성능상 이득이냐를 따져보았을 때, 같은 값을 가진 객체라고 해서 같은 메모리를 가질 것이라는 법칙은 없다.


더 나아가 True, False, None 이들 또한 객체라고 부르기에

```python
id(True)
>>> 4535273808

id(False)
>>> 4535273840

id(None)
>>> 4535374312
```

각자에 해당하는 id 값이 존재한다.

```python
def return_value(abc):
  print(abc)

return_value(3)
>>> 3
```

이때

```python
3 is return_value(3)
>>> 3 False
```

```python
id(return_value(3))
>>> 4341563880

id(3)
>>> 4341846608
```


### 위치인자 묶음

함수에 위치인자로 주어진 변수들의 묶음은 `매개변수명` 으로 사용할 수 있다. 관용적으로 `*args`를 사용한다.
> 우리가 따로 위치인자의 수를 정해주지 않았을 때 사용

```python
def print_args(*args):
  print(args)

print_args(3,5,124,'hello')
>>> (3,5,124,'hello')
# 튜플 형식으로 출력된다.
```

### 키워드 인자 묶음

함수에 키워드인자로 주어진 변수들의 묶음은 `**매개변수명`으로 사용할 수 있다. 관용적으로 `**kwargs`를 사용한다.

```python
def print_kwargs(**kwargs):
  print(kwargs)

print_kwargs(a='apple', b='banana', hi='123123123123')
>>> {'a':'apple', 'b':'banana', 'hi':'123123123123'}
```


### docstring

함수를 정의한 문서 역할을 한다.

함수 정의 후, 몸체의 시작부분에 문자열로 작성하며, 여러줄로도 작성가능하다.

```python
def print_args(*args):
  'args로 전달된 위치인자들을 출력해준다.'
  print(args)

help(print_args)
>>> Help on function print_args in module __main__:

    print_args(*args)
      args로 전달된 위치인자들을 출력해준다.

```

### 함수를 인자로 전달

파이썬에서는 함수 역시 다른 객체와 동등하게 취급되므로, 함수에서 인자로 함수를 전달, 실행, 리턴하는 형태로 프로그래밍이 가능하다.

* 'call func'를 출력하는 함수를 정의하고, 함수를 인자로 받아 실행하는 함수를 정의하여 첫 번째에 정의한 함수를 인자로 전달해 실행해보자.

```python
def print_call():
  print(call func)

def print_execute(f):
  f()

print_execute(print_call)
```

### 내부 함수

함수 안에서 또 다른 함수를 정의해 사용할 수 있다.

* 문자열 인자를 하나 전달받는 함수를 만들고, 해당 함수 내부에 전달받은 문자열을 대문자화해서 리턴해주는 내부 함수를 구현한다. 문자열을 전달받는 함수는 내부함수를 실행한 결과를 리턴하도록 한다.

```python
def execute(a):
  def upper(b):
    return b.upper()
  return upper(a)

execute('asdasd')
>>> 'ASDASD'
```

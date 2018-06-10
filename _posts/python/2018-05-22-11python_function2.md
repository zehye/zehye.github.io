---
layout: post
title: python 함수 - scope, lambda, closure, decorator, generator
category: python
tags: [python, scope, lambda, closure, decorator, generator ]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 `python` 함수에 대해 설명합니다.

<hr>

## 스코프 (영역)

파이썬에서는 코드 작성 시, 각 함수마다 독립적인 스코프(영역)을 가진다.

메인 프로그램(현재 동작하는 프로그램의 최상위 위치)의 영역은 전역영역(`global scope`)라고 하며, 전역 스코프 내부에서 독립적인 영역을 갖고 있는 경우에는 지역영역(`local scope`)라고 부른다.

```python
champion = 'Lux'

def show_global_champion():
  print('show_global_champion : {}'.format(champion))

show_global_champion()
>>> show_global_champion : Lux

print('print champion : {}'.format(champion))
>>> print champion: Lux
```

```python
champion = 'Lux'

def show_global_champion():
  print('show_global_champion : {}'.format(champion))

def change_global_champion():
  print('before change_global_champion : {}'.format(champion))
  champion = 'Ahri'
  print('after change_global_champion : {}'.format(champion))

show_global_champion()
>>> show_global_champion: Lux

change_global_champion()
>>> UnboundLocalError: local variable 'champion' referenced before assignment
```

change_global_champion 함수에서 오류를 볼 수 있다.

첫번째 코드에서는 `champion`변수가 함수의 로컬 스코프에 존재하지 않기 때문에 글로블스코프에서 해당 변수를 찾아 출력했으나, 다음 코드에서는 내부에 또다른 `champion`변수가 존재하기 때문에 할당하기 전인 변수를사용한 것으로 판단하여 프로그램에서 오류를 발생시킨다.

```python
id(show_global_champion)
>>> 4378671712

id(change_global_champion)
>>> 4375901320
```

뿐만 아니라, 각 영역에 해당하는 데이터들은 `locals()`함수를 사용해 확인할 수 있으며, 전역 영역의 데이터들은 `globals()`함수를 사용한다.

```python
champion = 'Lux'

def show_global_champion():
  print('show_global_champion : {}'.format(champion))

def change_global_champion():
  print(locals())
  champion = 'Ahri'
  print('after change_global_champion : {}'.format(champion))

show_global_champion()
>>> show_global_champion : Lux

change_global_champion()
>>> {}
    after change_global_champion : Ahri

```

즉, 함수는 독립적인 영역으로 위에서 정의한 것을 아래에서도 똑같이 정의 받지 못한다. 그래서 정의 받지 못하는 함수는 로컬의 값을 정의받는데, 그때 그것을 확인하는 방법은 `locals()`을 사용한다. 이를 사용해서 보면 두번째에 사용한 lux의 값은 {}으로 아무것도 받지 못했음을 알 수 있다.


### 스코핑 롤

스코프는 지역(local), 전역(global)외에도 내장(bulit-in)영역이 존재하며, 내장영역이 가장 바깥, 그 내부에 전역, 그 내부의 지역순으로 정의된다.

분리된 영역에서, 외부 영역에서는 내부 영역의 데이터를 사용할 수 없지만, 내부 영역에서는 자신의 외부 영역에 있는 데이터를 참조할 수 있다. (반대의 경우에는 함수의 인자로 데이터를 전달한다.)

즉 위의 예를 보면 `champion = 'Lux'`에서는 아래 `show_global_champion`함수의 데이터를 사용할 수 없지만, `show_global_champion`에서는 `champion = 'Lux'`데이터를 참조할 수 있다.


### 내장함수와 내장영역

`print`, `dict` 등 지정하지 않고 사용했던 내장 함수들은 위 스코핑 룰의 내장 스코프에 존재하는 함수들이다. 전역스코프의 `__builtin__` 변수에 할당되어 있으며, 전역 스코프에서는 해당 변수의 내부를 참조할 수 있도록 파이썬이 시작될 때 자동으로 처리된다.

확인시 `dir`함수를 사용하며, dir함수는 해당 객체가 사용 가능한 속성 및 함수들을 리스트 형태로 나타내준다.


# 로컬 스코프에서 글로벌 스코프의 변수를 사용

```python
champion = 'Lux'

def change_global_champion():
  champion = 'Ahri'
  print('after change_global_champion : {}'.format(champion))

change_global_champion()
>>> after change_glabal_champion : Ahri

print('print global champion : {}'.format(champion))
>>> print global champion : Lux
```

이 경우에는 `show_global_champion`함수와는 다르게 `change_glabal_champion`함수는 `champion`변수에 새로운 값(`Ahri`)을 대입한다. 만약 로컬 스코프에서 글로벌 스코프의 변수를 변경해야 한다면, 해당 변수가 로컬 스코프에서 생성되는 것이 아닌 글로벌 영역에 이미 존재하는 값을 가용함을 명시해주어야 한다.


```python
champion = 'Lux'

def change_glabal_champion():
  global champion
  champion = 'Ahri'
  print('after change_glabal_champion : {}'.format(champion))


change_glabal_champion
>>> after change_global_champin : Ahri

print('print global champion : {}'.format(champion))
>>> print global champion : Ahri
```

파이썬에서는 한 스코프에서 동일한 이름을 가진 두 스코프의 변수를 사용할 수 없음을 기억해야 한다.


### 내부 함수에서의 로컬 스코프 (nonlocal)

```python
champion = 'Lux'

def local1():
  global champion
  champion = 'Ahri'
  print('local1 locals() : {}'.format(champion))

  def local2():
    champion = 'Exreal'
    print('local2 locals() : {}'.format(locals()))

  local2()

print('global champion : {}'.format(champion))
>>> global champion : Lux

local1()
>>> local1 locals() : Ahri
    local2 locals() : {'champion': 'Ezreal'}
```

이렇듯 로컬 스코프 내부에 또 다른 로컬 스코프가 존재할 수 있다.

전역 스코프가 아닌, 자신의 바로 바깥 영역의 로컬 스코프(자신보다 한 단계 위의 로컬 스코프)의 데이터를 참조하고자 한다면, `nonlocal`키워드를 사용한다.

```
champion = 'Lux'

def local1():
  champion = 'Ahri'
  print('local1 locals() : {}'.format(champion))

  def local2():
    nonlocal champion
    champion = 'Ezreal'
    print('local2 locals() : {}'.format(champion))

  local2()
  print('local1 locala() : {}'.format(locals()))

print('global champion : {}'.format(champion
>>> global champion : Lux
local1()
>>> local1 locals() : Ahri
    local2 locals() : {'champion': 'Ezreal'}
    local1 locals() : {'local2': <function local1.<locals>.local2 at 0x10ea617b8>, 'champion': 'Ezreal'}
```



### 글로벌 키워드와 인자 전달의 차이

인자로 전달한 경우

```python
global_level = 100

def level_add(value):
  value += 30
  print(value)

level_add(global_level)
>>> 130

print(global_level)
>>> 100

# 글로벌 레벨이라는 변수는 100이라는 변수를 참조하고 있다.
# 레벨 애드라는 함수에 벨류라는 파라미터를 가지고 여기서 글로벌 레벨을 전달했는데,
# 이 벨류가 같은 곳을 가리키게 된다. (100)
# 그 다음 밸류에 +30이 되어 메모리에는 130이 생기고 레벨에드는 100과의 연결이 끊기고 130에게 가게 되는 것이다.

# 그러면서 id값도 바뀌게 된다.
# 함수가 130을 가리키게 되면 그 130이라는 메모리는 이미 함수가 종료했으니 사라지고
# 여전히 100만 남기 때문에 프린트 글로벌 레벨은 100이 그대로 실행된다.
```

### 글로벌 키워드를 사용한 경우

```python
global_level = 100

def level_add():
  global global_level
  global_level += 30
  print(global_level)

print(global_level)
>>> 100

level_add()
>>> 130

print(global_level)
>>> 130
```

즉, 인자로 전달한 경우, 같은 객체를 가리키는 글로벌 변수 `global_level`과 매개변수 `value`가 존재한다. 이때, 매개변수인 `value`의 값을 변경하는 것은 `global_level`에는 영향을 주지 않는다.

`global`키워드의 경우 둘은 같은 변수이다.



### 람다함수

한 줄 짜리 표현식으로 이루어지며, 반복문이나 조건문 등의 제어문은 포함될 수 없다. 또한 함수이지만 정의/호출이 나누어져 있지 않으며 표현식 자체가 바로 호출된다.

```python
lambda <매개변수> : <표현식>
```

```python
# 함수의 정의
>>> def multi(x):
...   return x*x
...

# 함수의 호출
>>> multi(5)
25

# 람다함수의 사용
>>> (lambda x : x*x)(5)
25

# 람다함수를 사용해 함수 정의
>>> f = lambda x : x*x
>>> f(5)
25
```

```python
def multi(x):
  return x*x


def get_one_value_and_execute_function(value, f):
  return f(value)

get_one_value_and_execute_function(5, multi)
>>> 25

# 위의 함수는
get_one_value_and_execute_function(5, lambda x: x*x)
# 같은 역할을 한다.

```

### 클로져 (closure)

함수가 정의된 환경을 말하며, 파이썬 파일이 여러개일 경우 각 파일은 하나의 `모듈` 역할을 하고, 각 `모듈`은 독립적인 환경을 가진다. 독립된 환경은 각자의 영역을 전역 영역으로 사용한다.

```python
level = 0

def outer():
  level = 50

  def inner():
    nonlocal level
    level += 30
    print(level)

  return inner
  # inner 함수 자체를 return

f = outer()

f() = 53
f() = 56
f() = 59
...
...
f2 = outer()

f2() = 53
f2() = 56
f2() = 59
...
...

type(f)
>>> function

print(f)
>>> <function outer.<locals>.inner at 0x110da9730>

```


### 데코레이터 (decorator)

함수를 받아 다른 함수를 반환하는 함수, 예로들면 기존에 존재하던 함수를 바꾸지 않고 전달된 인자를 보기위한 디버깅 `print` 함수를 추가한다던가 하는 기능을 할 수 있다.

1.기능을 추가할 함수를 인자로 받음
2.데코레이터 자체에 추가할 기능을 함수로 정의
3.인자로 받은 함수를 데코레이터 내부에서 적절히 호출
4.위 2가지를 행하는 내부 함수를 반환


```python
def print_arguments(original_function):

  def decorated_function(*args, **kwargs):
    print(f'args: {args}')
    print(f'kwargs: {kwargs}')
    return original_function(*args, **kwargs)

  return decorated_function

```

```python
def square(x):
  return x**2

square(5)
>>> 25

@print_arguments
def double(x):
  return x*2

double(2)
>>> args:(2,)
    kwargs:{}
    4

def half(x):
  print('input : {}'.format(x))
  return x//2

half(8)
>>> input : 8
    4

```


### 제너레이터 (generator)

제터레이터는 함수는 파이썬의 시퀀스 데이터를 생성하는 데 사용된다. 실제 시퀀스 데이터와 다른점은, 시퀀스 전체를 가지고 있는 것이 아니라 시퀀스 데이터를 생성하기 위한 어떠한 루틴만을 가지고 있는 것이다.

이 방식을 택했을 때의 장점은, 전체 크기만큼의 메모리를 가지고 있는 시퀀스 데이터와는 달리 메모리를 적게 사용할 수 있다.

제너레이터는 마지막으로 호출한 위치(항목)을 기억하고 있으며, 한 번 순회할 때 마다 그 다음 값을 반환한다.

제너테이터는 함수를 통해 만들어지고, 함수 내부의 반복문에서 `yield`키워드를 사용하면 제너레이터가 된다.


```python
def range_gen(num):
  i = 0
  while i < num:
    yield i
    i +=1

type(range_gen)
>>> function

g = range_gen(10)
type(g)
>>> generator

next(g)
>>> StopIteration: # 오류발생
```

```
for item in g:
  print(item)

0
1
...
9
```

저기서의 g는 0~9까지의 숫자를 가지고 있는게 아니라,
0~9까지 도는 루틴만을 가지고 있는 것이다.

한번 사용하면 다시는 돌지 않아서 generator를 다시 돌려주고 실행해야 한다.


### 실습
1.매개변수로 문자열을 받고, 해당 문자열이 red면 apple을, yellow면 banana를, green이면 melon을, 어떤 경우도 아닐 경우 I don't know를 리턴하는 함수를 정의하고, 사용하여 result변수에 결과를 할당하고 print해본다.

```python
def print_fruitname(a):
  if a == 'red':
    return 'apple'
  elif a == 'yellow':
    return 'banana'
  elif a == 'green':
    return 'melon'
  else:
    return 'I don't know'

```

```python
fruit_dict = {
    'red':'apple',
    'yellow':'banana',
    'green':'melon',
}

def what_fruit(color):
    return fruit_dict.get(color,'I don\'t konw')
```

2.1번에서 작성한 함수에 docstring을 작성하여 함수에 대한 설명을 달아보고, help(함수명)으로 해당 설명을 출력해본다.

```python
def print_fruitname(a):
  '이 함수는 과일의 색을 넣으면 해당 과일의 정보를 알려줍니다.'
  if a == 'red':
    return 'apple'
  elif a == 'yellow':
    return 'banana'
  elif a == 'green':
    return 'melon'
  else:
    return 'I don't know'

help(print_fruitname)

```

3.한 개 또는 두 개의 숫자 인자를 전달받아, 하나가 오면 제곱, 두개를 받으면 두 수의 곱을 반환해주는 함수를 정의하고 사용해본다.

```python
def print_number(a,b=None):
  if b is None:
    return a ** 2
  else:
    return a*b

# 더 간단한 방법
def square_or_multi_default_parameter(a, b=None):
  if b:
    return a ** 2
  return (a*b)

# 더더 간단한 방법
def square_or_multi_default_parameter(a, b=None):
    return a*b if b else a**2

# 혹은 위치인자 묶음을 통해서 구하는 방법
def squeare_of_multi_positional_args(*args): #위치 인자 묶음 사용
    if len(args) == 2:
        return args[0] * args[1]
    return args[0] **2

# 더 사람이 이해하기 쉽게 표현해보자면
def squeare_of_multi_positional_args(*args): #위치 인자 묶음 사용
    args_length = len(args)
    # if args_length > 2 or args_length <1:
    if not (args_length == 1 or args_length ==2):
        raise ValueError('숫자는 1개 또는 2개로 입력해주세요')
    if len(args) == 2:
        return args[0] * args[1]
    return args[0] **2


# 튜플, 언패킹으로 푸는 방법
def squeare_of_multi_positional_args(*args):
    if len(args) == 1:
        # 튜플 언패킹 시, 좌-우변 모두 'tuple'형태로 만들어야 한다. (',' 반드시 있어야 한다.)
        args1, = args
        return args1 ** 2
    elif len(args2) == 2:
        arg1, arg2 == args
        return args1 * args2

print_number(4)
>>> 16

print_number(4,6)
>>> 24
```

4.두 개의 숫자를 인자로 받아 합과 차를 튜플을 이용해 동시에 반환하는 함수를 정의하고 사용해본다.

```python
def print_number(a,b):
  return (a+b, absa-b)


print_number(9,5)
>>> (14,4)
```

5.위치인자 묶음을 매개변수로 가지며, 위치인자가 몇 개 전달되었는지를 print하고 개수를 리턴해주는 함수를 정의하고 사용해본다.

```python
def print_number(*args):
  print('args의 갯수 : {}'.format(args))
  return len(args)


print_number(3,4,5,6,7)
>>> 'args의 갯수 : 5'
    5
```


6.람다함수와 리스트 컴프리헨션을 사용해 한 줄로 구구단의 결과를 갖는 리스트를 생성해본다.

```python
[(lambda x,y : '{} x {} = {}'.format(x,y,x*y)) (x,y) for x in range(2,10) for y in range(1,10)]
```

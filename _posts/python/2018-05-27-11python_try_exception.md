---
layout: post
title: python try-exception(예외처리)
category: python
tags: [python, try, exception, finally, raise]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 `python` 예외처리에 대해 설명합니다.

<hr>

## 예외처리

오류가 발생하면 프로그램은 에러를 출력하며 강제종료 되거나, 원하지 않는 동작을 한다.

이러한 오류를 안전하게 처리하고, 바로 강제종료 되지 않고 오류 발생 후 처리할 루틴을 실행하고자 할 때 예외처리를 사용한다.

**가장 기본적인 형태**

```python
try:
	시도할 코드
except:
	에러가 발생했을 경우 실행할 코드
```

```python
list1 = list(range(3))
list1
>>> [0,1,2]

try:
  list1[4]
except:
  print('리스트 범위를 벗어났습니다.') # IndexError

>>> 리스트 범위를 벗어났습니다.
```

**여러가지 예외를 구분할 경우**

```python
try:
	시도할 코드
except <예외 클래스1>:
	에러클래스 1에 해당할 때 실행할 코드
except <예외 클래스2>:
	...
except <예외 클래스3>:
	...
```

```python
sample_list = list('apple')
sample_dict = {'red':'apple', 'yellow':'banana'}

try:    
    print(sample_list[2])
    print(sample_dict['red'])

except IndexError as e:
    print('해당 리스트가 존재하지 않습니다')
    print(e) # 어떤 오류가 발생했는지를 알아볼 수 있다.
    #print(e.args)
    #print(e.with_traceback)

except KeyError:
    print('해당 리스트가 존재하지 않습니다')

else: # 예외가 발생하지 않을경우 실행됨
    print('예외없이 잘 끝났음!')

finally: # 예외가 있든 없든 실행됨
    print('어쨋든 끝났음!')
# 위에서 오류가 나면 아래 오류는 그냥 넘어가버리고

>>> p
apple
예외없이 잘 끝났음!
어쨋든 끝났음!

```

**예외사항을 변수로 사용할 경우**

```python
try:
	시도할 코드
except <예외클래스> as <변수명>:
	<변수명>을 사용한 코드

#위에서 사용했던
except IndexError as e:
    print('해당 리스트가 존재하지 않습니다')
    print(e)

```


### try ~ else

`else`문은 `try`이후 예외가 발생하지 않을 경우 실행된다.

```python
try:
	시도할 코드
except:
	예외 발생시 실행 코드
else:
	예외가 발생하지 않았을 시 실행할 코드

# 위에서 사용했던
else:
    print('예외없이 잘 끝났음!')

```

### try ~ finally

`finally`문은 `try`이후 예외가 발생하건, 하지 않건 무조건 마지막에 실행된다.

```python
finally:
    print('어쨋든 끝났음!')
```

### 예외를 발생시키기 - 실습

```python
class ExpectIntException(Exception):
    def __init__(self, value):
        self.value = value
        self.msg = f'숫자가 전달되어야 합니다. (받은 값: {value})'


    def __str__(self):
        return self.msg

def square(x):
    if not isinstance(x, int): # 특정 인덱스가 인스턴스인지 알아보는 경우
        raise ExpectIntException(x)
    return x ** 2
```
```python
while True:
    try:
        value = input('숫자를 입력해주세요: ')
        print(square(value))
        break  
    except ExpectIntException as e:
        print(e)
    except ValueError as e:
        print(e)
```

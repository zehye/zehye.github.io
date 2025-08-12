---
layout: post
title: python module과 package
category: Python
tags: [python, module, package]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 `python` 모듈과 패키지에 대해 설명합니다.

<hr>

## module?

지금까지 셸에서 진행한 실습의 경우, 하나의 모듈 내부에서 작업한 것으로 취급된다.

파이썬 파일은 각각 하나의 모듈로 취급되며, 실행이나 함수의 정의, 단순 변수의 모음 등등 여러 역할을 한다.


### 모듈 불러오기 (import)

기본 모듈에서, 부가적인 기능들을 불러와 사용하는 형태

```python
# python -> projects -> module이라는 폴더를 만들어
# module 내 vim을 통해 shop.py game.py lol.py 를 만든다.

# shop.py 내에는 buy_item이라는 함수를 만들어 실행시키도록 하고
def buy_item():
  print('Buy item!')

buy_item()

# game.py 내에는 play_game이라는 함수를 만들어 실행시키도록 한다.
def play_game():
  print('Play game!')

play_game()

# 그리고 lol.py 내부에는 shop.py와 game.py를 호출 시킨뒤 실행시키게 한다.
import shop
import play

print(' = Turn on game')
shop.buy_item()
game.play_game()
```

```
# 그러고 나서 셸에서 lol.py를 실행시키면
Play game!
Buy item!
=Turn on game
Play game!
Buy item!
```
이렇게 출력이 되는데, 그 이유는 lol.py에서 import를 하는 순간 shop.py와 game.py에 있는 함수가 각각 출력이 되는 것이다. (그렇기 때문에 중복으로 출력이 되는 것)

이를 막아야 하는데, 우리가 사실 모듈을 import하는 경우가 상당히 많은데, 이 모듈이 독자적인 행동을 갖고 싶은 경우가 많다.

우리가 일반적으로 셸에서

```
python shop.py
>>> Buy item!

python game.py
>>> Play game!
```

이렇게 나오는데, import를 하는 경우에는 이렇게 나오지 않게 하고 싶은 것

즉, 우리가 shop.py 혹은 game.py를 메인 프로그램으로 사용하는 경우에는 출력이 되지만(buy_item()와 play_game()이 실행되는 것), 단순 import를 하는 경우에는 그 안에 있는 함수만 가져오고 싶은 것

그럴 때 사용하는 것이 `__name__`이라는 변수이다.


### __name__

파이썬 인터프리터를 이용해 실행한 코드인지를 확인하여 단순히 `import`한 모듈의 경우 실행을 막는 방식으로 각 모듈은 자신의 이르믈 가지며, 모듈 이름은 모듈의 전역변수 `__name__`에서 확인할 수 있다.

파이썬 인터프리터가 실행한 모듈의 경우, `__main__`이라는 이름을 가진다.

따라서 `python <파일명>`으로 실행한 경우에만 동작할 부분은 `if`문으로 감싸준다. 즉, 지금 자신, 이 모듈의 이름이 메인이라면 그 파일을 실행했구나라는 것을 파이썬이 알 수 있도록 해준다.

```python
def buy_item():
  print('Buy item!')

if __name__ == '__main__':
  buy_item()


def play_game():
  print('Play game!')

if __name__ == '__main__':
  play_game()

# 이후 셸에서 lol.py를 해보면
=Turn on game
Play game!
Buy item!
```

그럼 이제까지는 lol.py를 실행시키면 바로 게임을 키고, 게임을 하고, 아이템을 구매하게 했는데, 이제는 그게 아닌 어떤 특정 입력을 기다렸다가 그 입력에 대해 대응하는 방법을 알아보도록 하겠다.

코드를 조금 바꿔보기

```python
import game
import shop

def turn_on():
  print('= Turn on game')

  while True:
    choice = input('뭐할래? \n 1: 상점가기 \n 2: 게임시작하기 \n 3: 종료 \n 입력: ')

if __name__ == '__main__':
  turn_on()
```

그런데 이 방식을 하면 아무리 input에 값을 넣어도 실행이 되지 않는다.
따라서 각 wiled문에서 벗어날 수 있도록 input에 값을 만들어 준다.


```python
import game
import shop

def turn_on():
  print('= Turn on game')

  while True:
    choice = input('뭐할래? \n 1: 상점가기 \n 2: 게임시작하기 \n 3: 종료 \n 입력: ')

    if choice == '1':
      shop.buy_item()
    elif choice == '2':
      game.play_game()
    elif choice == '3':
      break
    else:
      print('1,2,3 중 하나를 선택해주세요.')

if __name__ == '__main__':
  turn_on()
```

### 네임스페이스 (namespace)

각 모듈은 독립된 네임스페이스(이름 공간)을 가진다. 지난번에 클로져를 가진다고 까지는 이야기를 했는데, 근데 그 클로져를 사용하는 방법이 네임스페이스를 이용하는 방법이다.

메인으로 사용되고 있는 모듈이 아닌  `import`된 모듈의 경우, 해당 모듈의 네임 스페이스를 사용해 모듈 내부의 데이터에 접근한다.

즉, `lol.py`에서 사용한 `import game`은 `game.py`에 있는 `play_game`함수에 어떻게 접근하냐면, 이 `namespace`가 `import game`의 `game`이라는 이름을 갖는 것

그래서 `game.play_game()`을 실행시킬 수 있는 것이다.

만약 모듈명을 생략하고 싶다면 (`game.play_game()`이 아니라 `play_game()`이라고 하고 싶다면)

```python
# import game
from game import play_game

# 현재 스코프, 즉 네임 프로그램에 있는 함수 이름이랑, 다른 데서 import할 때 shop에서도 뭔가 play_game이랑 똑같은 무언가를 가져온다면 밑에 재정의한걸로 덮어씌워지니까 조심해야한다.

import shop

def turn_on():
  print('= Turn on game')

  while True:
    choice = input('뭐할래? \n 1: 상점가기 \n 2: 게임시작하기 \n 3: 종료 \n 입력: ')

    if choice == '1':
      shop.buy_item()
    elif choice == '2':
      # 해당 네임스페이스를 사용하지 않아도 된다.
      play_game()
    elif choice == '3':
      break
    else:
      print('1,2,3 중 하나를 선택해주세요.')

if __name__ == '__main__':
  turn_on()
```

그러면 이제 각각 똑같은 이름의 함수를 만들어 본다.

```python
def buy_item():
  print('Buy item!')

def shop_info()
  print('shop module')

if __name__ == '__main__':
  buy_item()


def play_game():
  print('Play game!')

def shop_info():
  print('game module')

if __name__ == '__main__':
  play_game()

# lol.py에서

# import game
from game import play_game,shop_info as game_info
from shop import shop_info as shop_info
import shop

game_info()
shop_info()

def turn_on():
  print('= Turn on game')

  while True:
    choice = input('뭐할래? \n 1: 상점가기 \n 2: 게임시작하기 \n 3: 종료 \n 입력: ')

    if choice == '1':
      shop.buy_item()
    elif choice == '2':
      # 해당 네임스페이스를 사용하지 않아도 된다.
      play_game()
    elif choice == '3':
      break
    else:
      print('1,2,3 중 하나를 선택해주세요.')

if __name__ == '__main__':
  turn_on()

```

```
game module
shop module
=Turn on game
뭐할래?
 1: 상점가기
 2: 게임시작하기
 3: 종료
 입력:
```

### 패키지

특별한 폴더를 뜻하는데, 지금까지 만든 모듈은 다 같은 라인에 있는데, 패키지로 만들게 되면 계층구조를 가질 수 있다. 모듈들을 하나의 패키지안에 몰아넣고 나중에 어떤 기능들을 하나의 패키지들을 몰아넣고 그 패키지에서 기능들을 가져다 쓸 수 있도록 한다.

패키지를 만들때는 패키지로 사용할 폴더에 `__init__.py`파일이 있어야한다.

```python
# 우리는 function이라는 파일을 만들어서 game.py와 shop.py를 해당 폴더에 이동하고
# 그 폴더 안에 __init__.py를 만든다. (touch __init__.py)

# 그러고 나서 lol.py를 실행하면 분명 module not found 에러가 뜬다.
# 그건 아래처럼 고치면 된다.

# import game
from function.game import play_game,shop_info as game_info
from function.shop import shop_info as shop_info
from function import shop

game_info()
shop_info()

def turn_on():
  print('= Turn on game')

  while True:
    choice = input('뭐할래? \n 1: 상점가기 \n 2: 게임시작하기 \n 3: 종료 \n 입력: ')

    if choice == '1':
      shop.buy_item()
    elif choice == '2':
      # 해당 네임스페이스를 사용하지 않아도 된다.
      play_game()
    elif choice == '3':
      break
    else:
      print('1,2,3 중 하나를 선택해주세요.')

if __name__ == '__main__':
  turn_on()

```


### __all__

패키지에 포함된 하위 패키지 및 모듈을 모두 불러올 때, `*`을 사용하면 모듈의 모든 식별자들을 불러온다. 이때, 각 모듈에서 자신이 `import`될 때 불러와질 목록을 지정하고자 한다면 `__all__`을 정의하면 된다.

패키지 자체를 `import`시에 자동으로 가져오고 싶은 목록이 있다면, 패키지의 `__init__.py`파일에 해당 항목을 `import`해주면 된다.

```python
from function.game import *
```


`*`을 해서 가져오되, 주지 않아도 될 것들이 있을 때 (쓸데 없는 import를 막기 위해), 내가 지정한 것만 import 하고 싶다면 `__all__`을 사용한다.

```
__all__ = (
  'play_game',
  'shop_info',
  )
```

이렇게 되면 all안에 있는 `play_game`과 `shop_info`함수만 가지고 오는 것을 의미한다.

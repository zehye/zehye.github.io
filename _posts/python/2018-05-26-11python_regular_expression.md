---
layout: post
title: python regular expression(정규표현식)
category: Python
tags: [python, regular-expression]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 `python` 정규표현식에 대해 설명합니다.

<hr>

## 정규표현식 (Regular Expression)

특정한 패턴에 일치하는 복잡한 문자열을 처리할 때 사용하는 기법

파이썬에서는 표준 모듈 `re`를 사용해 정규표현식을 사용할 수 있다.

```python
import re
```

### match : 시작(맨 처음)부터 일치하는 패턴찾기

```python
import re
source = 'Lux, the Lady of Luminosity'

m = re.match('Lux', source)
# match의 첫번째 인자에는 패턴이 들어가며, 두번째 인자에는 문자열 소스가 들어간다.
# match()는 소스와 패턴의 일치여부를 확인하며, 일치할 경우 match object를 반환한다.

print(m)
# <_sre.SRE_Match object; span=(0, 3), match='Lux'>

print(m.group())
>>> 'Lux'

# 만약
m = re.match('Lady', source)
print(m)
>>> None

#그런데
m = re.match('Lux, the Lady', source)
print(m.group())
>>> 'Lux, the Lady'
```

그런데 마지막 패턴 검색보다 훨씬 쉽게 문장을 불러오는 방법이 있다.

```python
m = re.match('.*Lady', source)
>>> 'Lux, the Lady'
```

여기서 `.`은 \n제외한 문자 1개, `*`은 해당 패턴이 0회 이상 올 수 있다는 의미를 지닌다.
즉, 문자 1개 (어떤 문자든) 그게 0회 이상 올 수 있다는 의미를 뜻한다.

0회 이상 올 수 있다는 말은 문자가 아무것도 안와도, 공백이 있어도 다 허용이 된다는 것이다.


### search : 첫번째 일치하는 패턴찾기

`*`없이 `Lady`하나만을 찾을 경우에는, `search()`을 사용한다.

```python
m = re.search('Lady', source)

print(m.group())
>>> 'Lady'
```


### findall : 일치하는 모든 패턴 찾기

`findall`을 사용하면 해당 문자가 리스트로 출력된다.

```python
re.findall('L',source)
>>> ['L', 'L', 'L']
```

만약 문자가 단순 `L`이 아니라 `L`뒤에 문자가 더 나오는 것을 찾고 싶다면

```python
re.findall('L.{2}', source)
>>> ['Lux', 'Lad', 'Lum']

# 혹은
re.findall('L..', source)
>>> ['Lux', 'Lad', 'Lum']
```

두 방법 모두 같은 결과물을 출력해준다.

### split : 패턴으로 나누기

문자열의 `split()`메서드와 비슷하지만 패턴을 사용할 수 있다.

```python
m = re.split('o', source)
m
>>> ['Lux, the Lady ', 'f Lumin', 'sity']
```

### sub : 패턴 대체하기

문자열이 `replace()` 메서드와 비슷하지만 패턴을 사용할 수 없다.

```python
m = re.sub('o','!',source)
m
>>> 'Lux, the Lady !f Lumin!sity'
```


### 정규표현식의 패턴 문자

패턴 | 문자
--- | ---
\d | 숫자
\D | 비숫자
\w | 문자
\W | 비문자
\s | 공백 문자
\S | 비 공백 문자
\b | 단어경계 (\w와 \W의 경계)
\B | 비단어 경계

각 해당 패턴의 기능을 알아보고 싶다면

```python
import string
printable = string.printable

print(re.findall('알아보고 싶은 패턴', printable))
```

### 정규표현식의 패턴 지정자 (Pattern specifier)

**`expr`은 정규 표현식을 말한다**

패턴 | 의미
--- | ---
abc | 리터럴 `abc`
expr | expr
expr1 | expr2 | expr1 또는 expr2
. | `\n`을 제외한 모든 문자
^ | 소스 문자열의 시작(문장의 시작)
$ | 소스 문자열의 끝(문장의 끝)
expr? | 0또는 1회의 expr
expr* | 0회 이상의 최대 expr(반복)
expr+ | 1회 이상의 최대 expr
expr*? | 0회 이상의 최소 expr
expr+? | 1회 이상의 최소 expr
expr{m} | m회의 expr
expr{m,n} | m에서 n회의 최대 expr ({0,} = `*`, {1,} = `+`, {0,1} = `?`)
expr{m,n}? | m에서 n회의 최소 expr
[abc] | a or b or c
[^abc] | not(a or b or c)
expr1(?=expr2) | 뒤에 expr2 오면 expr1에 해당하는 부분
expr1(?!expr2) | 뒤에 expr2 오지 않으면 expr1에 해당하는 부분
(?<=expr1)expr2 | 앞에 expr1 오면 expr2에 해당하는 부분
(?<!expr1)expr2 | 앞에 expr1 오지 않으면 expr2에 해당하는 부분


### 매칭 결과 그룹화

정규표현식 패턴 중 `괄호` 로 둘러싸인 부분이 있을 경우, 결과는 해당 괄호만의 그룹으로 저장된다.

`match`객체의 `group()`함수는 매치된 전체 문자열을 리턴하며, `group()`함수는 지정된 그룹 리스트를 리턴해준다. `group(0)`은 `group()`과 같은 동작을 하며, `group(숫자)`는 매치된 `숫자`번째의 그룹요소를 리턴해준다.


`(?P<name>expr)`패턴을 사용하면 매칭된 표현식 그룹에 이름을 붙여 사용할 수 있다.

```python
m = re.search(r'(?P<before>\w+)\s+(?P<was>was)\s+(?P<after>\w+)', story)

m.groups()
m.group('before')
m.group('was')
m.group('after')
```


### 최소일치와 최대일치

```html
html = '<html><body><h1>HTML</h1></body></html>'
```

위 항목을

```python
re.findall(r'<.*>', html)
>>> ['<html><body><h1>HTML</h1></body></html>']

re.finadll(r'<.*?>',html)
>>> ['<html>', '<body>', '<h1>', '</h1>', '</body>', '</html>']

```

로 검색하면, `.*`표현식이 제일 처음으로 나오는 `>`에서 멈추는 것이 아니라, 맨 마지막 `>`까지 검색을 진행한다. `*`는 0회부터 최대로 패턴이 출력되는 것이니까.

그런데 `*`이나 `+`에 최소일치인 `?`를 붙여주면, 표현식 다음부분에 해당하는 문자열이 처음나왔을 때부터 그 부분까지만 일치시키고 검색을 마친다.


### 실습
1.`{m}`패턴지정자를 사용해서 `a`로 시작하는 4글자 단어를 전부 찾는다.

```python
re.findall(r'\ba\w{3}\b',story)
>>> ['also', 'able']
```

2.`r`로 끝나는 모든 단어를 찾는다.

```python
re.findall(r'\b\w+r\b',story)
>>> ['for', 'daughter', 'clear', 'engineer', 'after', 'over', 'for', 'her', 'her', 'her', 'for', 'her', 'her', 'her', 'favor', 'However', 'for', 'her', 'her', 'her', 'brother', 'her', 'for']
```

3.a,b,c,d,e중 아무 문자나 3번 연속으로 들어간 단어를 찾는다.

>ex) b[eca]me

```python
re.findall(r'\w*[abcde]{3}\w*',story)
re.findall(r'\b\w*[abcde]{3}\w*\b',story)
>>> ['advanced', 'became', 'made', 'embrace', 'became', 'deep']
```

4.`re.sub`를 사용해서 `,`로 구분된 앞/뒤 단어에 대해 앞단어는 대문자화 시키고, 뒷단어는 대괄호로 감싼다. 이 과정에서, 각각의 앞/뒤에 `before`, `after`그룹 이름을 사용한다.

```python
re.findall(r'(\w+)\s*,\s*(\w+)', story)
>>> [('Crownguards', 'the'),
 ('service', 'Luxanna'),
 ('daughter', 'and'),
 ('matured', 'it'),
 ('Somehow', 'she'),
 ('prodigy', 'drawing'),
 ('government', 'military'),
 ('Magic', 'she'),
 ('gift', 'something'),
 ('skills', 'the'),
 ('conflict', 'earning'),
 ('However', 'reconnaissance'),
 ('people', 'Lux'),
 ('Legends', 'where')]

re.sub(r'(\w+)\s*,\s*(\w+)', r'\1[\2]', story)
>>> "Born to the prestigious Crownguards[the] paragon family of Demacian service[Luxanna] was destined for greatness. She grew up as the family's only daughter[and] she immediately took to the advanced education and lavish parties required of families as high profile as the Crownguards. As Lux matured[it] became clear that she was extraordinarily gifted. She could play tricks that made people
...


def repl_function(m):
  return '{} [{}]'.format(m[1].upper(), m[2])

re.sub(r'(\w+)\s*,\s*(\w+)', repl_function ,story)

# 혹은
re.sub(r'(\w+)\s*,\s*(\w+)', (lambda m : '{} [{}]'.format(m[1].upper(), m[2])),story)

# 그룹 지어서 만들기

re.sub(r'(?P<before>\w+)\s*,\s*(?P<after>\w+)',(lambda m : '{} [{}]'.format(m['before'].upper(), m['after']))

>>> "Born to the prestigious CROWNGUARDS [the] paragon family of Demacian SERVICE [Luxanna] was destined for greatness. She grew up as the family's only DAUGHTER [and] she immediately took to the advanced education and lavish parties required of families as high profile as the Crownguards. As Lux MATURED [it] became clear that she was extraordinarily gifted. She could play tricks that made people believe they
...
```

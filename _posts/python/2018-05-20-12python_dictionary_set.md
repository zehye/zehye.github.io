---
layout: post
title: python dictionary, set
category: python
tags: [python, dictionary, set]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 `python` 딕셔너리와 셋에 대해 설명합니다.
---

<hr>

## 딕셔너리 (dictionary)

key-value 형태로 항목을 가지는 자료구조

### 딕셔너리 생성

```
>>> empty_dict1 = {}
>>> empty_dict2 = dict()
>>> champion_dict = {
... 'Lux': 'the Lady of Luminosity',
... 'Ahri': 'the Nine-Tailed Fox',
... 'Ezreal': 'the Prodigal Explorer',
... 'Teemo': 'the Swift Scout',
... }
```

### 형변환

```
>>> sample = [[1,2], [3,4], [5,6]]
>>> dict(sample)
{1: 2, 3: 4, 5: 6}
```

### 항목 찾기/추가/변경

```
>>> champion_dict['Lux']
'the Lady of Luminosity'
>>> champion_dict['Sona'] = 'Maven of the Strings'
>>> champion_dict['Lux'] = 'Demacia'
```

### 결합 (update)

```
>>> com_dict = {}
>>> com_dict.update(champion_dict)
>>> com_dict.update(item_dict)
>>> com_dict
```

### 삭제 (del)

```
>>> del com_dict['Doran\'s Blade']
```

이외에도 전체삭제 `clear`, `in`으로 True, False값을 반환하는 키 값을 검색해볼 수 있다.


> keys()는 모든 키를 얻고, values()는 모든 값을 얻고, items()는 모든 키와 값을 튜플로 반환하여 얻을 수 있다.



## 셋 (set)

셋은 키만 있는 딕셔너리와 같으며, 중복된 값이 존재할 수 없다.

### 셋 생성

```
>>> empty_set = set()
>>> champions = {'lux', 'ahri', 'ezreal'}
```

### 형변환

```
>>> set('ezreal')
{'e', 'z', 'a', 'l', 'r'}
# 중복된 값은 하나로 나타난다.

>>> set(champion_dict)
{'Ahri', 'Lux', 'Ezreal', 'Sona', 'Teemo'}
# 딕셔너리는 키만 남는다.
```

### 집합연산

연산자 | 설명
---- | ----
| | 합집합(union)
& | 교집합(intersection)
- | 차집합(difference)
^ | 대칭차집합(exclusive)




### 실습

1.`apple`은 `사과`, `banana`는 `바나나`, `cherry`는 `체리`의 key-value를 갖는 `fruits`라는 이름의 사전을 만든다.

```
fruits = { 'apple': '사과', 'banana': '바나나', 'cherry':'체리'}
```

2.`fruits`를 `Set`으로 만들어 `fruits_set`변수에 할당한다.

```
fruits_set = set(fruits)
```

3.`fruits_set`에 `durian`이 존재하는지 확인한다.

```
'durian' in fruits_set
>>> False
```

4.`fruits`사전에서 `apple`키에 해당하는 값을 출력한다.

```
fruits['apple']
>>> '사과'
```

5.`girlgroups`라는 이름의 2차원 사전을 만들고 출력해본다.

* 최상위 키는 `girlsday`와 `redvelvet`이 있으며, 각각 자식으로 사전을 갖는다.

* `girlsday`키의 자식사전에는 `korean`과 `members`키가 있으며, 각각 `'걸스데이'`라는 문자열과 `['민아', '혜리', '소진', '유라']`라는 리스트를 갖는다.

* `redvelvet`키의 자식사전에도 `korean`과 `members`키가 있으며, 각각 `'레드벨벳'`이라는 문자열과 `['아이린', '슬기', '웬디', '조이', '예리']`라는 리스트를 갖는다.

```
girlgroups = {
  'girlsday': {
    'korean': '걸스데이',
    'members': '민아, 혜리, 소진, 유라'.split(',')
  },
  'redvelvet': {
    'korean': '레드벨벳',
    'members': '아이린, 슬기, 웬디, 조이, 예리'.split(',')
  }
}
```

6.`girlgroups`사전의 최상위 키 목록을 출력해본다.

```
girlgroups.keys()
>>> dict_keys(['girlsday', 'redvelvet'])
```

7.`girlgroups['girlsday']`의 모든 키를 출력해본다.

```
girlgroups['girlsday'].keys()
>>> dict_keys(['korean', 'members'])
```

8.`girlgroups['redvelvet']`의 모든 값을 출력해본다.

```
girlgroups['redvelvet'].values()
>>> dict_values(['레드벨벳', ['아이린', ' 슬기', ' 웬디', ' 조이', ' 예리']])
```

9.`x = {1,2,3,4,5,6,8}`, `y={4,5,6,9,10,11}`, `z={4,6,8,9,7,10,12}`일 때,

* x,y,z의 교집합에 해당하는 숫자는?

```
x&y&z
>>> {4, 6}
```

* y,z의 교집합이며 x에는 속하지 않는 숫자는?

```
(y&z)-x
>>> {9, 10}
```

* x에만 속하고 y,z에는 속하지 않는 숫자는?

```
x-(y|z)
>>> {1, 2, 3}
```

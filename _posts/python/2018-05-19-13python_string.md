---
layout: post
title: python 문자열
category: python
tags: [python, string]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 `python` 문자열에 대해 설명합니다.

<hr>
## 문자열

### 작은 따음표와 큰 따음표를 혼용해서 쓰는 방법

```
# 기본적으로는 ''와 "" 둘다 사용 가능하다.
>>> '안녕하세요'
>>> "안녕하세요"

# ''안에 "" 사용이 가능하고 ""안에 '' 사용이 가능하다.
>>> '안녕하세요 속에 "안녕하세요"'

#'''는 여러줄에 걸친 문자열을 나타낼 때 사용한다.
>>> '''안녕하세요
...
... 안녕안녕 나는나는나는
...
... 누구게요?
... 누구인지 맞춰보세요! '''

```

### 이스케이프 문자

이스케이프 문자 | 설명
----------- | -----
\t | 탭 (tab)
\n | 줄바꿈
\\ | \(역슬래시) 입력
\' | 작은따옴표 (') 입력
\" | 큰따옴표 (") 입력


### 인덱스 연산

문자열에서 문자를 추출할 때 가장 왼쪽은 0, 가장 오른쪽은 -1

```
>>> lux = '빛으로 강타해요!'
>>> lux[0]
'빛'
>>> lux[-1]
'!'
```

> 그런데 문자열은 불변이기에, 인덱싱한 부분에 새 값을 대입할 수는 없다.

`lux[0]='손'`

 위 같은 대입은 불가능하다.


### 슬라이스 연산

`[start:end:step]` 형식을 사용

* [:] : 처음부터 끝까지
* [start:] : start부터 마지막까지
* [:end] : 처음부터 end까지
* [start:end] : start부터 end까지
* [start:end:step] : start부터 end까지, step만큼 뛰어넘는다.


### 길이

내장함수 `len`을 사용



### 문자열 나누기 (slpit)

```
>>> girlsday = "민아,유라,소진,혜리"
>>> girlsday.split(',')
['민아', '유라', '소진', '혜리']
```


### 문자열 결합 (join)

```
>>> girlsday_list = girlsday.split(',')
>>> girlsday_str = ', '.join(girlsday_list)
>>> print(girlsday_str)
민아, 유라, 소진, 혜리
```

### {} format

```
>>> fruits = '사과, 바나나, 멜론'.split(',')
>>> colors = '빨강, 노랭, 초록'.splt(',')

>>> for item in fruits:
      print('fruits: {}, colors: {}'.format(item, colos[fruits.index(item)]))

fruits: 사과, colors: 빨강
fruits: 바나나, colors: 노랑
fruits: 멜론, colors: 초록
```

###  f 표현법

`f'{변수 또는 표현식}'`


```
# 기본 형태
apple = '사과'
banana = '바나나'
train = '기차'

f'{apple}은 맛있어 맛있으면 {banana} {banana}는 길어 길면 {train}]
```


### 실습

1.여러 줄의 텍스트를 `multi_lines`변수에 할당하고, `print()`함수로의 출력과 인터프리터의 자동 출력(변수명 입력)을 비교해보시오.

```
multi_lines = '''안녕하세요
안
녕
하

세 요~ '''

>>> print(multi_lines)
>>> multi_lines
```

2.`str1`, `str2`변수에 각각 문자열을 할당하고, 두 변수를 결합해 `str3`변수에 할당해보시오.

```
str1 = '안녕'
str2 = '하세요'

>>> str3 = str1 + str2
>>> str3
'안녕하세요'
```

3.`str1`변수에 `*` 연산자를 사용한 결과를 출력해보시오.

```
>>> str3 * 3
'안녕하세요안녕하세요안녕하세요'
```

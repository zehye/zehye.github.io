---
layout: post
title: 통계 - 평균, 중앙값, 최빈값 
category: Statistics
tags: [Statistics]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 평균(Mean)

모든 값의 합을 전체 개수로 나눈 값 

$$
\bar{x} = \frac{1}{n} \sum_{i=1}^{n} x_i
$$

- $\bar{x}$: 평균 (x-bar라고 읽음)
- $n$: 데이터의 개수 (전체 항목 수)
- $x_i$: i번째 데이터 값
- $\sum$: 시그마 기호, 모든 데이터를 더하라는 뜻


```python
#A반 점수 모음
A_score = [60, 70, 90, 85, 100, 75, 65, 80, 95, 55 ]

#A반 합
a_sum = sum(A_score)

#A반 평균
a_sum/len(A_score)  # 77.5
```

평균은 계산이 쉽고 직관적이어서 데이터를 대표할 수 있는 값으로 많이 사용되지만, 어떤 특정한 극단적인 값에 영향을 받이 받는다는 단점도 존재한다. 


## 중앙값(Median)

데이터를 크기순으로 정렬할때, 중앙에 위치한 값 

```python
# 원본 데이터
data1 = [30, 10, 20]

# 정렬
sorted_data1 = sorted(data1)  # [10, 20, 30]

# 중앙값 (홀수 개수)
n1 = len(sorted_data1)
median1 = sorted_data1[n1 // 2]

print("정렬된 데이터:", sorted_data1)
print("중앙값:", median1)

# [10, 20, 30]
# 20
```

그러나 데이터가 홀수개인 경우는 어떻게 할까?

```python
# 원본 데이터
data2 = [40, 20, 10, 30]

# 정렬
sorted_data2 = sorted(data2)  # [10, 20, 30, 40]

# 중앙값 (짝수 개수 → 가운데 두 수의 평균)
n2 = len(sorted_data2)
middle_left = sorted_data2[n2 // 2 - 1]
middle_right = sorted_data2[n2 // 2]
median2 = (middle_left + middle_right) / 2

print("정렬된 데이터:", sorted_data2)
print("중앙값 계산:", middle_left, "+", middle_right, "/ 2 =", median2)
# [10, 20, 30, 40]
# 25.0
```

$$
\text{중앙값} = \frac{20 + 30}{2} = 25
$$


이러한 중앙값은 극단값(이상치)의 영향을 거의 받지 않고, 데이터가 한쪽으로 너무 치우쳐 있거나 이상치가 있을 경우 평균보다 더 현실적인 대표값을 가질 수 있다는 장점이 있다. 그러나 계산에 정렬이 반드시 필요하기 때문에 평균보다 더 많은 연산을 사용하게 되고, 모든 데이터 값을 반영하지 않고 단순히 위치만 고려한다는 단점 또한 존재한다. 

따라서 데이터에 이상치가 존재하거나 분포가 비대칭인 경우 평균보다 중앙값을 대표값으로 사용하는 것이 적절하다. 


## 최빈값(Mode)

데이터 중 가장 자주 나타나는 값 > 가장 많이 나온 값 

```python 
fruits = ["사과", "바나나", "사과", "포도", "사과", "딸기", "바나나"]

freq = {}  # 빈 딕셔너리 

for fruit in fruits:
    if fruit in freq:
        # freq안에 과일 이미 존재하면 +1
        freq[fruit] += 1
    else:
        # freq안에 과일 없으면 1
        freq[fruit] = 1

# 결과 > freq = {'사과': 3, '바나나': 2, '포도': 1, '딸기': 1}

max_count = 0
mode_fruit = None

for key in freq:
    # 1. 사과(3)가 0보다 크면
    if freq[key] > max_count:
        # max_count = 3
        max_count = freq[key]
        # mode_fruit = 사과
        mode_fruit = key
# 사과(3) -> max_count 보다 큰 값은 안나올테니 
print("최빈값:", mode_fruit)
# 사과 
```

라이브러리를 통해서도 최빈값을 찾아보자

```python 
from collections import Counter

fruits = ["사과", "바나나", "사과", "포도", "사과", "딸기", "바나나"]
counter = Counter(fruits)
most_common = counter.most_common(1)[0][0]  # 가장 많이 등장한 값

print("최빈값:", most_common)
# 사과 
```

이러한 최빈값은 데이터에서 가장 흔하게 나타나는 값을 파악하기가 쉽다. 하지만 최빈값이 존재하지 않는 경우가 있을수도 있고(모든값이 한번만 나오는 경우), 여러개의 최빈값이 존재할 수도 있다는 것을 알고 있어야한다. 

## **대표값 정리 비교**

| 항목  | 의미           | 이상치 영향 | 적합한 데이터    |
|  -|  -| - | - |
| 평균  | 전체 합 ÷ 개수    | 있음     | 수치형 데이터    |
| 중앙값 | 정렬 후 가운데 위치  | 적음     | 비대칭 분포     |
| 최빈값 | 가장 자주 나타나는 값 | 없음     | 범주형 또는 수치형 |
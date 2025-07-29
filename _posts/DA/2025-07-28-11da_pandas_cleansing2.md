---
layout: post
title: Pandas 데이터 전처리 다양한 실습 
category: DA
tags: [DA]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 데이터 전처리

### where

특정 조건을 만족하는 행들의 값만 필터링하고 나머지 행은 Nan값으로 대치 

혹은 Nan 대신 대치될 값을 적어줄 수도 있음 (각 열들의 값을 기준으로 더 복잡한 조건을 지정할 수 있음)

```python 
# 나이를 0~100범위를 두고 나머지에 대해서는 NaN처리
df['age'] = df['age'].where(df['age'].between(0,100), np.nan)
```


### descibe

기술통계량(평균, 표준편차, 최대, 최소값 등)을 보여주는 메소드 

```python
df.describe()
```


### value_counts

각 값의 개수(등장 횟수)를 카운팅

- 가장 많은 빈도수부터 내림차순 정렬 
- 리턴값은 시리즈 
- 기본적으로는 Nan값은 제외하지만 `dropna=False`를 적용하면 Nan값이 포함

```python
# A열에서 가장 많이 등장한 값 추출
df.value_counts('A').idxmax()[0]
# A열의 최빈값 등장 횟수
df.value_counts('A').max()

# 그반대는 아래와 같다
df.value_counts('A').idxmin()[0]
df.value_counts('A').min()
```


### match 

특정 단어와 완전 일치하는 데이터 필터링 (contains로는 겹쳐버리는 문자가 있으면 걸러내지 못하는 데이터를 match를 통해 걸러낼 수 있다)

정규표현식을 전달해, 해당 정규식에 해당하는 인덱스 반환 


```python 
reg = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+.[a-zA-Z]{2,}$'
df['email'] = df['email'].where(df['email'].str.match(reg, na=False))
```


### rename

컬럼의 이름을 바꾸는 함수

1. 전체 컬럼 이름 한꺼번에 바꾸기
2. 개별적으로 바꾸기 > 이떄 사용하는 것이 rename 

우선 전체 컬럼을 한꺼번에 바꾸는 방법은 아래와 같다.

```python 
# 전체 컬럼명을 확인 
df.columns  # Index(['a', 'b', 'c'])

# 바꿀 컬럼명 대입
df.columns = ['에이', '비', '씨']
df.columns  # Index(['에이', '비', '씨'])
```

위 경우는 전체 컬럼을 한번에 바꿀때에만 가능하다. 

이제 rename을 사용해 개별 컬럼명을 변경해보자.

```python 
# 전체 컬럼명을 확인 
df.columns  # Index(['a', 'b', 'c'])

df.rename(columns = {'a':'에이', 'b':'비'}, inplace=True)
df.columns  # Index(['에이', '비', 'c'])
```


### mode

행/열의 최빈값을 구하는 함수(최빈값이 여러개일 경우 모두 표시) 
> 최빈값: 가장 많이 등장한 값 

- df.mode(axis=0, numeric_only=False, dropna=True)

```python 
# 최빈값 찾기 
df['A'].mode()[0]
```


### select_dtypes

데이터프레임 내 포함 혹은 제외할 데이터 타입을 지정할 수 있는 함수

```python 
# 포함할 데이터타입 
df = df.select_dtypes(include='object')
# 제외할 데이터타입
df = df.select_dtypes(exclude=['int64', 'object'])
```
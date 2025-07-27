---
layout: post
title: Pandas 데이터 전처리 
category: DA
tags: [DA]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 데이터 전처리

데이터 분석을 위해 수집한 데이터를 분석에 적합한 형태로 가공하는 과정 

- 결측치 처리
- 이상치 처리
- 중복 제거
- 오타 및 불일치 수정
- 데이터 형식 정리
- 범주 통일 및 정규화
- 공백 및 특수문자 제거


**전처리 하는 이유**

1. 분석의 정확도를 높이기 위해
2. 모델의 훈련을 높이기 위해(머신러닝 훈련시 노이즈나 이상치는 성능저하를 일으키기에)
3. 일관된 인사이트 도출을 가능하게 하기 위해


다음과 같이 데이터프레임 하나를 생성해보자 

```python
df = pd.DataFrame({
    'name': [' Alice ', 'Bob', 'Charlie', 'Alice ', 'Bob'],
    'age': [25, np.nan, 35, 25, 999],
    'gender': ['F', 'M', 'M', 'F', 'male']
})
```


### 공백제거

name 안에는 ' Alice ', 'Alice '가 있어 해당 데이터의 공백을 제거해줘야 한다.

```python
df['name'] = df['name'].str.strip()
```


### 중복값 제거 

```python
df = df.drop_duplicates()
```


### 결측치 제거 및 채우기

**1. 이상치를 결측치로 변경**

age의 np.nan 결측치 처리

```python 
df['age'] = df['name'].replace(999, np.nan)
# or
df.loc['age'] = df['age'].replace(float(999.0), np.nan)
```

```python
df['gender'] = df['gender'].replace({
    'male': 'M',
    'female': 'F'
})
```


**2. 결측치 채우기**

```python 
df['age'] = df['age'].fillna(df['age'].mean())
```


------

다른 예시를 통해 전처리를 해보자

이렇게 데이터 전처리를 위해 일일히 하나하나 확인해서 처리하는것은 어려울 수 있다.

1. distinct 메소드를 통해 유니크한 값을 도출해낸다
2. 내가 원하는 범주로 카테고리화 한다
3. 필요없는 혹은 변경되어야할 값들을 딕셔너리화 혹은 수정한다


```python 
df = pd.DataFrame({
    '이름': ['홍길동', '김영희', '이철수'],
    '나이': [25, np.nan, 32],
    '성별': ['M', 'F', None]
})
```


### 결측치 확인 

각 행에 결측치의 합을 확인 

```python
df.isnull().sum()
```


### 결측치 삭제

```python
df = df.dropna()
```


### 결측치 평균값으로 채우기

나이에 비어있는 값을 평균값으로 채우기 

```python
df['나이'] = df['나이'].fillna(df['나이'].mean())
```


### 결측치 넣기

성별에 비어있는 값을 채워넣고, 데이터의 통일성 맞춰주기

```python 
df['성별'] = df['성별'].replace({
    'male': 'M',
    'female': 'F',
    None = '알수 없음'
})
```


------

## 전처리를 위한 기본 실습 


### 데이터 시리즈 두개를 생성해보자

```python
menu = pd.Series(['간계밥', '감자탕', '김나돈', '냉삼'])
price = pd.Series([10000, 15000, 8000, 25000])
```


### 두개의 시리즈를 합쳐 하나의 데이터 프레임을 생성

```python
df = pd.DataFrame({'메뉴': menu, '가격': price})
```

### 데이터프레임에 원산지 컬럼을 추가해보자

```python
df['원산지'] = ['한국', '미국', '중국', '일본']
```


### 파일 저장하고 불러와보기

```python
# 파일저장
df.to_csv('menu.csv')

# 파일 불러오기
temp_df = pd.read_csv('파일경로')
```


### 파일 내 데이터를 원하는 개수만큼 뽑아보기

```python 
temp_df.head(3)  # 앞에서 3개
temp_df.tail(3)  # 뒤에서 3개
temp_df.sample(3)  # 샘플 3개 
```


### 데이터 정보 확인하기

```python
df.info()
```

------


다음을 위해 중복값이 있는 데이터를 생성해보자

```python
df_car = pd.DataFrame({
    "car":['Sedan','SUV','Sedan','SUV','SUV','SUV','Sedan','Sedan','Sedan','Sedan','Sedan'],
    "size":['S','M','S','S','M','M','L','S','S', 'M','S']
})
```


### 유니크한 데이터의 개수 확인

```python
df_car.nunique()
```


### 유니크한 항목의 종류

```python
df_car['car'].unique()
df_car['size'].unique()
```


### 중복값 확인

```python
df_car.duplicated()
```

or 특정컬럼을 대상으로 확인

- keep = True: 전체를 대상
- subset = ['']: 특정컬럼을 대상

```python 
df_car.duplicated(subset=['size'])
```


### 자료형 변환 

```python 
data = {
    "메뉴":['아메리카노', '카페라떼', '카페모카', '카푸치노', '에스프레소', '밀크티', '녹차'],
    "가격":[4500.0, 5000.0, 5500.0, 5000.0, 4000.0, 5900.0, 5300.0],
    "칼로리":['10', '110', '250', '110', '20', '210', '0'],
}

df.info()
```

``` 
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 7 entries, 0 to 6
Data columns (total 3 columns):
 #   Column  Non-Null Count  Dtype  
---  ------  --------------  -----  
 0   메뉴      7 non-null      object 
 1   가격      7 non-null      float64
 2   칼로리     7 non-null      int64  
dtypes: float64(1), int64(1), object(1)
memory usage: 300.0+ bytes
```

이떄 칼로리 문자형을 숫자로 변환해보자(+ 가격도 숫자로)

```python
df['칼로리'] = df['칼로리'].astype(int)
df['가격'] = df['가격'].astype(int)
```

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 7 entries, 0 to 6
Data columns (total 3 columns):
 #   Column  Non-Null Count  Dtype 
---  ------  --------------  ----- 
 0   메뉴      7 non-null      object
 1   가격      7 non-null      int64 
 2   칼로리     7 non-null      int64 
dtypes: int64(2), object(1)
memory usage: 300.0+ bytes
```

### 컬럼 추가와 삭제

```python
# 할인가 컬럼 추가
dis = 0.3
df['할인가'] = df['가격']*(1-dis)

# 첫번째 행 삭제
df.drop(1, axis=0, inplace=True)

# 할인가 컬럼 삭제
df.drop('할인가', axis=1, inplace=True)
```


### 컬럼 생성 후 결측값 대입

```python 
df['원산지'] = np.nan
```

### 결측값 채우기

```python
df.loc[0, '원산지'] = '콜롬비아'
df.loc[4, '원산지'] = '콜롬비아'
df.loc[2, '원산지'] = '과테말라'
df.loc[3, '원산지'] = '과테말라'
```


### 인덱스 번호 리셋하기

```python
df.reset_index(drop=True)
```


### 인덱스 정렬

```python
df.sort_index()

# 역으로 정렬
df.sort_index(ascending=False)
```

### 특정 열 기준 정렬

```python
df.sort_value(by='메뉴', ascending=True)
```


### 정렬값 저장

```python
df.sort_value(asceding=True, inplace=Ture)
# or
df_sort = df.sort_value(asceding=True)
```


### 데이터 조건 확인

가격이 5000원이상인 것 확인 

```python
df5000 = df['가격'] >= 5000
```

가격이 5000원 이상이면서 원산지가 과테말라인것

```python
df_g = df['원산지'] == '과테말라'
df[df5000 & df_g]
```

### isin

메뉴에 녹차가 있는지 확인 > 정확한 단어로 확인

```python
df_n = df['메뉴'].isin(['녹차'])
```


### contains

'이름' 컬럼에서 "카"를 포함하는 행 찾기 > 하나라도 포함되어있는 단어를 확인 

```python 
filtered_df = df[df['메뉴'].str.contains('카', na=False)]
```

### startswith, endswith

```python
df[df['메뉴'].str.startswith('녹')]
df[df['원산지'].str.endswith('라')]
```

혹은 인덱스로 찾는 방법

```python 
df[df['메뉴'].str[0] == '녹'] 
```


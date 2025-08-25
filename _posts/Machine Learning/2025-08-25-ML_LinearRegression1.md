---
layout: post
title: 머신러닝 - 선형 회귀분석 모델(Linear Regression)
category: Machine Learning
tags: [Machine Learning]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

# 머신러닝 모델의 훈련단계

```
0. 라이브러리 불러오기(import)
1. 데이터 불러오기
2. 전처리(데이터 클렌징, 정규화, 범주화 데이터 처리)
3. 데이터 스플릿/분리(train, test)
4. 모델 정의(내가 어떤 모델을 사용할지, 하이퍼파라미터 설정)
5. 모델 훈련(`.fit()` > 파라미터를 훈련시켜서 함수통을 만든다) 
6. 훈련 결과 확인(정확도, 시각적 방법을 통해 결과 확인)
```

## 선형 회귀분석(Linear Regression)

- 선형 회귀: 독립변수와 종속변수 간 선형관계를 모델링하는 통계학적 기법
- 머신러닝의 가장 기본적이면서도 중요한 알고리즘 중 하나
- 독립변수의 갯수에 따라 단순/다중 선형회귀로 나뉨 


## 단순 선형회귀(Simple Linear Regression)

독립변수가 하나인 경우

```
y = β₀ + β₁x + ε
```

- y: 종속변수 (예측하려는 값)
- x: 독립변수 (설명변수)
- β₀: 절편 (intercept)
- β₁: 기울기 (slope)
- ε: 오차항 (error term)


### 라이브러리 불러오기

```python
import numpy as np
import pandas as pd

import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
```

### 데이터 불러오기 

```python 
# 독립변수 > 공부시간 
X = np.array([1,2,3,4,5]).reshape((-1,1))
# 종속변수 > 시험점수
y = np.array([5,20,14,32,22])  
```

### 모델 정의 > LinearRegression()  

```python 
# 모델이라는 변수에다가 정의 
model = LinearRegression()  
```

### 모델 훈련 > fit()

```python 
# 그래프에서 선을 긋는 것 
# y와 y^의 잔차를 줄이는 것이 목표 
# y^ (와이 헷)은 머신러닝 결과로 나오는 예상치를 말함
model.fit(X,y)
```


### 훈련 결과 확인

```python 
w = model.coef_[0]
b = model.intercept_

print(w,b)
# 4.6000000000000005 4.800000000000001
print(f'회귀식: y={w:.2f}x + {b:.2f}')
# 회귀식: y=4.60x + 4.80
```


### 모델 예측 > predict()

```python 
X1 = np.array([2,2,4,4,5]).reshape((-1,1))

# 만들어진 통계모델로 예측 
y_pred = model.predict(X1)  
y_pred
```



## 다중 선형회귀(Multiple Linear Regression)

독립변수가 여러개인 경우

```
y = β₀ + β₁x₁ + β₂x₂ + ... + βₚxₚ + ε
```

- y: 종속변수 (예측하려는 값)
- x: 독립변수 (설명변수)
- β₀: 절편 (intercept)
- β₁: 기울기 (slope)
- ε: 오차항 (error term)

### 라이브러리 불러오기 

```python 
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score
```

### 데이터 불러오기 & train/test 분리 

f(x)가 y가 되기 위해 학습해야하는 정보가 train이고 잘배웠는지 결과를 보는 것이 test

- x1: 시험 공부시간
- x2: 조명
- x3: 수면시간
- x4: 과제 참여도

```python 
X = np.array([[5,2,3],[10,3,5],[15,5,8],[7,2,4]])
y = np.array([10,20,30,40])

# test_size: 몇대몇으로 분리할 것이냐 > 0.2 : test에 20%, train에 80%
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```


### 모델 정의

```python 
model = LinearRegression()
```


### 모델 훈련

```python 
# 어떤 데이터가 들어가는 지 체크! 
model.fit(X_train, y_train)
```

### 모델 결과 확인

```python 
w = model.coef_
b = model.intercept_

print(w,b)
# [ 12.         -43.33333333   6.        ] 18.666666666666718
```


### 모델 예측 

```python 
model_pred = model2.predict(X_test)
model_pred

# array([38.66666667])
```

이때 우리의 목표는 예측할 수 있는 함수통을 만드는 것이다! 


### MSE

```python 
MSE = mean_squared_error(y_test, model2_pred)
MSE

# 348.4444444444457
```


---

## MSE, MAE, RMSE란?

- MSE:잔차 제곱의 합
- MAE: 잔차 평균의 합
- RMSE: 평균 제곱근 오차 > MSE에 루트씌운 값

**MSE, MAE 모두 작을수록 좋음!**

R^ = 1 - {(실제값 - 예측값)^의 합 /(실제값 - 전체 예측값의 평균)^ 의 합}



<center>
<figure>
<img src="/assets/post-img/ML/2.png" alt="" width="70%">
</figure>
</center>
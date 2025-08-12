---
layout: post
title: 회귀모델(Linear Regression)이란?
category: Statistics
tags: [Statistics]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

# 데이터에 따른 분류

입력(x) - 출력(y)

- 수치 - 수치: 회귀모델
- 수치 - 범주(이진): 로지스틱회귀모델
- 범주 - 수치: t test, ANOVA
- 범주 - 범주: 카이제곱검정

---
모수적/비모수적
- 모수적: 데이터가 정규분포를 따름
- 비모수적: 정규분포에 대한 가정이 거의 없음 > 설명력이 떨어짐


## 단순 회귀모델(Simple Linear Regression)?

하나의 독립 변수(X)와 하나의 종속 변수(Y)사이의 **선형 관계**를 모델링하는 회귀 분석

$$
y = \beta_0 + \beta_1 x + \varepsilon
$$

- $y$: 종속 변수 (예측하고 싶은 값)
- $x$: 독립 변수 (입력 변수)
- $\beta_0$: 절편 (Intercept)
- $\beta_1$: 기울기 (Slope) — $x$가 1 증가할 때 $y$가 얼마나 증가하는지
- $\varepsilon$: 오차 (실제값과 예측값의 차이)



### 시각적 설명

**직선 하나로 데이터를 설명**

- 산점도 위에 가장 잘 맞는 직선을 그리는 것이 목표
- 직선은 **최소제곱법(Least Squares)**으로 구함 → 오차제곱합을 최소화


### 단순 회귀의 가정

| 가정 항목 | 설명                                |
| ----- | --------------------------------- |
| 선형성   | X와 Y 사이에 선형 관계가 있어야 함             |
| 독립성   | 관측치 간의 오차는 서로 독립적이어야 함            |
| 등분산성  | 오차의 분산은 일정해야 함 (Homoscedasticity) |
| 정규성   | 오차는 정규분포를 따라야 함                   |


### 장점과 단점

| 장점         | 단점                      |
| ---------- | ----------------------- |
| 단순하고 해석 쉬움 | 변수 하나만 고려 → 실제 상황 반영 부족 |
| 시각화가 용이함   | 비선형 관계는 설명 불가           |
| 빠르게 계산됨    | 이상치에 민감                 |

---

### 단순 vs 다중 회귀 차이

| 항목    | 단순 회귀                     | 다중 회귀                                              |
| ----- | ------------------------- | -------------------------------------------------- |
| 독립 변수 | 1개                        | 2개 이상                                              |
| 수식    | $y = \beta_0 + \beta_1 x$ | $y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \cdots$ |
| 설명력   | 제한적                       | 더 많은 요인 반영 가능                                      |



```python 
# y = Q("quality")
# x = Q("fixed acidity") + Q("volatile acidity") ...  
# > 다중회귀모델 
Rformula = 'Q("quality") ~ Q("fixed acidity") + Q("volatile acidity") + Q("citric acid") \
+ Q("residual sugar") + Q("chlorides") + Q("free sulfur dioxide") + Q("total sulfur dioxide") \
+ Q("density") + Q("pH") + Q("sulphates") + Q("alcohol")'

regression_result = ols(Rformula, data=wine).fit()   
# .fit(): 모델통을 만들겠다 >(= 파라미터 훈련) 훈련되는 파라미터가 존재 > wine data
# ols().fit() > 선형회귀모델 구현 > 최적의 fit을 찾아나간다(모델) 
regression_result.summary()  # > Rsquared 값이랑 등등을 볼 수 있게 됨
```
```
OLS Regression Results
Dep. Variable:	Q("quality")	R-squared:	0.292
Model:	OLS	Adj. R-squared:	0.291
Method:	Least Squares	F-statistic:	243.3
Date:	Fri, 01 Aug 2025	Prob (F-statistic):	0.00
Time:	06:47:52	Log-Likelihood:	-7215.5
No. Observations:	6497	AIC:	1.445e+04
Df Residuals:	6485	BIC:	1.454e+04
Df Model:	11		
Covariance Type:	nonrobust		
coef	std err	t	P>|t|	[0.025	0.975]
Intercept	55.7627	11.894	4.688	0.000	32.447	79.079
Q("fixed acidity")	0.0677	0.016	4.346	0.000	0.037	0.098
Q("volatile acidity")	-1.3279	0.077	-17.162	0.000	-1.480	-1.176
Q("citric acid")	-0.1097	0.080	-1.377	0.168	-0.266	0.046
Q("residual sugar")	0.0436	0.005	8.449	0.000	0.033	0.054
Q("chlorides")	-0.4837	0.333	-1.454	0.146	-1.136	0.168
Q("free sulfur dioxide")	0.0060	0.001	7.948	0.000	0.004	0.007
Q("total sulfur dioxide")	-0.0025	0.000	-8.969	0.000	-0.003	-0.002
Q("density")	-54.9669	12.137	-4.529	0.000	-78.760	-31.173
Q("pH")	0.4393	0.090	4.861	0.000	0.262	0.616
Q("sulphates")	0.7683	0.076	10.092	0.000	0.619	0.917
Q("alcohol")	0.2670	0.017	15.963	0.000	0.234	0.300
Omnibus:	144.075	Durbin-Watson:	1.646
Prob(Omnibus):	0.000	Jarque-Bera (JB):	324.712
Skew:	-0.006	Prob(JB):	3.09e-71
Kurtosis:	4.095	Cond. No.	2.49e+05
```

> R-squared: 0.292의 의미

1. R-squared는 결정계수의 의미로 모델의 설명력을 의미
2. 0과 1사이의 값
3. 1에 가까울수록 설명력이 좋다
    - 보통 0.7 이상이면 설명력이 좋다, 유의미하다고 해석함 
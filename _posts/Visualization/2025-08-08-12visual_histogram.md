---
layout: post
title: Matplotlib 사용해보기(히스토그램 hist)
category: Visualization
tags: [Visualization]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Matplotlib 

```python 
import matplotlib.pyplot as plt
```


### hist 히스토그램

- 바 차트와 비슷
    - 차이점: 가로축에 수치형 데이터가 들어감 (바차트는 범주형이었음)


```python
# 시험 점수 데이터 (100명) 생성
np.random.seed(42)
scores = np.random.normal(75, 15, 100)  # 평균 75점, 표준편차 15
scores = np.clip(scores, 0, 100)  # 0-100점 범위로 제한 / clip> 제한하는 함수
```

```python 
# 히스토그램 구현
plt.figure(figsize=(5,3))
# bins: 데이터를 구획하는 숫자 > 데이터를 몇 구획으로 나눌것인가
# 많이할수록 더욱 세분화되어 보임
plt.hist(scores, bins=10,
         color='red',
         alpha=0.5,
         edgecolor='red',
         linewidth=1,
        #  density=True
         )
plt.title('시험 점수 분포')
plt.xlabel('점수')
plt.ylabel('학생수')
plt.show()
```

<left>
<figure>
<img src="/assets/post-img/Visualization/15.png" alt="" width="30%" height = "30%">
</figure>
</left>


### 통계선 추가해보기

통계션을 추가해서 많이 사용 > 평균, 중앙값 > `.axvline(값)`

```python
plt.figure(figsize=(5,3))
plt.hist(scores, bins=10,
         color='red',
         alpha=0.5,
         edgecolor='red',
         linewidth=1
         )

mean_score = np.mean(scores)
median_score = np.median(scores)

# .axvline > x축 통계선 추가 
plt.axvline(mean_score, color='purple', linestyle='--', label=f'평균: {mean_score:.2f}점')
plt.axvline(median_score, color='black', linestyle=':', label=f'중앙값: {median_score:.2f}점')
plt.legend(fontsize=8, loc='upper left')

plt.title('시험 점수 분포')
plt.xlabel('점수')
plt.ylabel('학생수')
plt.show()
```

<left>
<figure>
<img src="/assets/post-img/Visualization/16.png" alt="" width="30%" height = "30%">
</figure>
</left>


### 밀도 히스토그램 

- y축이 카운트 > 히스토그램
- x축이 비율 > 밀도 히스토그램

```
- 두개의 시각화 표현
- 밀도 히스토그램 구현(density=True)
```

```python 
plt.figure(figsize=(6,4))

plt.subplot(1,2,1)
plt.hist(scores, bins=10,
         color='red',
         alpha=0.5,
         edgecolor='red',
         linewidth=1,
        #  density=True
         )
plt.title('시험 점수 분포')
plt.xlabel('점수')
plt.ylabel('학생수')

plt.subplot(1,2,2)
plt.hist(scores, bins=10,
         color='red',
         alpha=0.5,
         edgecolor='red',
         linewidth=1,
         density=True  # 밀도 히스토그램이 표현됨
         )
plt.title('시험 점수 분포')
plt.xlabel('점수')
plt.ylabel('학생수')


plt.show()
```

<left>
<figure>
<img src="/assets/post-img/Visualization/18.png" alt="" width="30%" height = "30%">
</figure>
</left>




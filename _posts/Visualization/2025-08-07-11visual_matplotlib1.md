---
layout: post
title: Matplotlib 사용해보기(막대차트 bar, barh)
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

기본 데이터는 아래와 같다.

```python
# 간단한 매출 데이터 만들기
months = ['1월', '2월', '3월', '4월', '5월', '6월']
sales = [100, 120, 90, 140, 160, 180]

# DataFrame으로 만들기
df = pd.DataFrame({
    '월': months,
    '매출': sales
})
```


### Bar (bar)

- 일반적으로 가로축> `범주`, 세로축> `값`
- 데이터간의 크기를 비교하는데에 주로 사용됨
- 굉장히 기본적이고 많이 사용하는 시각화 중 하나
- 최대 3개의 정보를 표현가능(가로축, 세로축 색 지정 기준)
- 격자 나누기 기능을 사용해서 여러 컬럼 다룰수 있기는 함 (추가적인 정보를 표현하고 싶을때)



```python 
# plt는 matplotlib의 약어, 형태의 사이즈(가로, 세로)를 지정 
plt.figure(figsize=(4,2))

# bar chart 구현 > 가로, 세로축에 들어갈 데이터 지정 
plt.bar(df['월'], df['매출'])

# 제목 설정
plt.title('월별 매출 변화')
# y축 라벨 설정
plt.ylabel('매출(만원)') 
# x축 라벨 설정
plt.xlabel('2024년')

plt.show()
```

<left>
<figure>
<img src="/assets/post-img/Visualization/1.png" alt="" width="30%" height = "30%">
</figure>
</left>



### 가로축 Bar (barh)

- 바차트에서 가로축 정보는 정상형태일때 정보
- 세로축: `범주`, 가로축: `수치`
- 카테고리명이 길때 혹은 카테고리가 많을때
- 순위를 강조하고 싶을때 사용


```python 
plt.figure(figsize=(8,2)) 
plt.barh(df['월'], df['매출'])  

plt.title('월별 매출 변화')
plt.xlabel('매출(만원)')
plt.ylabel('2024년')
plt.show()
```

<left>
<figure>
<img src="/assets/post-img/Visualization/2.png" alt="" width="40%" height = "30%">
</figure>
</left>



### 데이터 정렬 후 차트 생성해보기

```python 
# 데이터 정렬
sales_data = list(zip(months, sales))  # 데이터 zip 통해 묶어줌
sales_data.sort(key=lambda x: x[1], reverse=False)  # 내림차순 정렬
#key = lambda x: x[1] 는 [('1월', 100), ('2월', 120), ('3월', 90), ('4월', 140), ('5월', 160), ('6월', 180)] 에서 뒤에 숫자를 기준으로 정렬한다!!

# 데이터를 다시 x축, y축에 넣을 것을 정리
months_s = [x[0] for x in sales_data]
sales_s = [x[1] for x in sales_data]

plt.figure(figsize=(8,2)) 
plt.barh(months_s, sales_s)  
plt.title('월별 매출 변화')
plt.xlabel('매출(만원)') 
plt.ylabel('2024년') 
plt.show()
```


### Bar Chart 이모저모

1. 차트 배경 색상 지정 > **plt.fugure(facecolor=)r**

```python 
plt.figure(figsize=(4,2), facecolor='y')
```

2. 막대 색상 지정 > **plt.bar(color=)**

```python 
plt.bar(months_s, sales_s, color='skyblue')
```

3. 원하는 막대의 색상 지정 

```python
# sales_s의 최댓값에만 색상 지정 
color = ['red' if x == max(sales_s) else 'blue' for x in sales_s]
plt.bar(months_s, sales_s, color=color)

# or
# 100만원 넘으면 빨간색, 100만원 이하면 파란색
def color_div(value):
    if value > 100:
        return 'red'
    else:
        return 'blue'

color = [color_div(x) for x in sales_s]
plt.bar(months_s, sales_s, color=color)
```

4. 막대 위에 값 표시하기

```python 
plt.figure(figsize=(4,2))
bars = plt.bar(months_s, sales_s)  # bars > 변수로 설정해주고 아래서 받기
plt.title('월별 매출 변화')
plt.ylabel('매출(만원)')
plt.xlabel('2024년')

# plt.text > 그래프 위에 글자를 덧씌우는 것
# get_x(): 왼쪽 모서리 좌표 출력
# get_width(): 가로 좌표
# get_height(): 세로 좌표
for bar, value in zip(bars, sales_s):
    # get_x(): 왼쪽 모서리 좌표 출력
    plt.text(bar.get_x()+bar.get_width()/2, bar.get_height(), f'{value}', ha='center', va='bottom')
    plt.ylim(0,200)

plt.show()
```

5. 막대 조절

```python 
edge = ['black' if x > 100 else 'pink' for x in sales_s]
plt.figure(figsize=(4,2))
plt.bar(months_s, sales_s, color=color,
        width=0.6,  # 너비 조절
        edgecolor=edge,  # 테두리 색상
        linewidth=2,  # 테두리 두께
        alpha=0.5  # 막대 불투명도
        )
plt.title('월별 매출 변화')
plt.ylabel('매출(만원)')
plt.xlabel('2024년')
plt.show()
```

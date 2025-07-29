---
layout: post
title: Pandas astype과 to_numeric의 차이 
category: DA
tags: [DA]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## to_numeric & astype

데이터 분석에서 수치형 데이터는 중요한 역학을 차지한다. 비수치형 데이터를 수치형 데이터로 변환하는 작업은 데이터 전처리 과정에서 자주 요구 된다. 이때 판다스는 이러한 변환을 위해 두가지 함수를 제공한다. 그게 바로 astype과 to_numeric이다.


<center>
<figure>
<img src="/assets/post-img/DA/33.png" alt="" width="60%">
</figure>
</center>


### astype

대부분의 자료형으로 변환을 지원하는 범용성을 갖추고 있어 수치형으로도 변환이 가능한 것으로 이때 astype을 사용하기 위해서는 모든 데이터가 해당 형식으로 변환할 수 있어야만 올바르게 작동된다.

즉 모든 데이터를 한꺼번에 수치형, 혹은 모든 자료를 어떠한 자료형으로 변환시킬 떄 사용할 수 있다는 것을 의미한다.



### to_numeric

astype보다 좀 더 유연하게 작동되어 변환할 수 없는 데이터를 자동으로 NaN처리하면서 변환할 수 있는 데이터만 수치형으로 변환해준다. 대규모 데이터 셋을 처리할 때 매우 유용하다. 

변환을 요구하는 데이터 셋 안에 '안녕'과 같은 오브젝트(문자형) 데이터가 있다면 이때는 astype을 통해 정수형 데이터로 변환시키지 못할 것이다. 이때 사용하는 것이 to_numeric함수이다. to_numeric을 사용하면서 `errors='coerce`를 입력하면 변환할 수 있는 데이터는 수치형으로 변환되고 변환할 수 없는 데이터는 NaN으로 처리된다. 

이떄 주의할 점은 두가지 있는데

1. to_numeric은 메서드로 사용할 수 없고 pd.to_numeric함수로 호출해야함
2. 데이터 프레임 전체에 사용불가능하며 시리즈에만 함수를 적용할 수 있음 

만약 데이터프레임 전체에 사용하고 싶다면 `apply`를 사용해 해결해야한다.


```python
# df라는 데이터프레임이 있다고 가정하고 
df = pd.dataFrame(data)

# 이런식으로 apply를 사용해 적용해야한다
df.apply(pd.to_numeric, errors='coerce')
```

<center>
<figure>
<img src="/assets/post-img/DA/34.png" alt="" width="60%">
</figure>
</center>

따라서 두 함수를 상황에 맞게 잘 선택해 사용하는 것이 중요하다! 
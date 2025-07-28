---
layout: post
title: Pandas missingno 라이브러리
category: DA
tags: [DA]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## missingno

결측치(null)값만 빠르게 확인하기 위한 시각화 라이브러리

결측치가 있는지 알아야 결측치를 없애든, 임의의 값을 넣든 할 수 있기 때문에 이를 위해 빠르게 시각화 하는 툴이다. 실무에서 반드시 사용해야하는 것은 아니지만, 업무를 더 잘하기 위해 사용하는 라이브러리 정도로 이해하고 넘어가면 좋을 듯 하다.


```python
import missingno as msno

msno.matrix(df)
```

<center>
<figure>
<img src="/assets/post-img/DA/31.png" alt="" width="60%">
</figure>
</center>


```python
import missingno as msno

msno.bar(df)
```


<center>
<figure>
<img src="/assets/post-img/DA/32.png" alt="" width="60%">
</figure>
</center>

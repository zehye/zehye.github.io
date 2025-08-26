---
layout: post
title: Matplotlib 한글깨짐 현상 폰트 설치 
category: Visualization
tags: [Visualization]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## Matplotlib 한글깨짐 현상

Matplotlib 으로 통계차트를 만들다보면 한글깨짐 현상이 발생한다.

이때 아래 코드를 실행해서 한글 폰트를 설치해주자.


```python 
# 시각화도구
import matplotlib.pyplot as plt
import matplotlib.font_manager as fm

# 한글 글씨 폰트 설치 
%%capture
!sudo apt-get install -y fonts-nanum
!sudo fc-cache -fv
!rm ~/.cache/matplotlib -rf

fm.fontManager.addfont('/usr/share/fonts/truetype/nanum/NanumGothic.ttf')
plt.rcParams['font.family'] = 'NanumGothic'
```


이때 만약 

```
UsageError: Line magic function `%%capture` not found.
``` 

이러한 에러가 떴다면 아래와 같이 해결해보자!
> %%capture 가 맨 윗단에 위치해서 실행되어야 하기 때문에 발생하는 것 

```python 
%%capture
# 시각화도구
import matplotlib.pyplot as plt
import matplotlib.font_manager as fm

# 한글 글씨 폰트 설치 
!sudo apt-get install -y fonts-nanum
!sudo fc-cache -fv
!rm ~/.cache/matplotlib -rf

fm.fontManager.addfont('/usr/share/fonts/truetype/nanum/NanumGothic.ttf')
plt.rcParams['font.family'] = 'NanumGothic'
```
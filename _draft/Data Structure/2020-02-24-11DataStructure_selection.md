---
layout: post
title: Data Structure, Selection and Crossover
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Selection

- 어떤 것이 교배 대상이 될것인가?
  - 자연선택
  - 얼마나 최적인가에 따라서 선택되어진다.
  - 가장 최적을 찾으려면 여러값을 해야한다.

- global optimum
  - 가끔 안좋은 데이터라도 우선 선택을 해본다.
  - inferior problem


### Selection step: Roulette wheel based

<center>
<figure>
<img src="/assets/post-img/DataStructure/47.png" alt="" width="50%">
</figure>
</center>


- worst 케이스보다 examined 케이스가 얼마나 작은가 > finteness 값이 0 이 될수 있음 > 우리에게 도움이 되는 worst 케이스가 있을 수 있음
- proportion에 의해서 골라지는 것
- worst soultion - exzmined soultion / k (우리 마음대로)

<center>
<figure>
<img src="/assets/post-img/DataStructure/48.png" alt="" width="50%">
</figure>
</center>

- 차가 크지 않는 경우 무엇이 선택되든 비슷해야함
- 만약 큰 경우에는 일부가 중점적으로 표현되어야함
- k가 늘어날 수록 결과적으로 worst 가 선택되는 비중이 적어짐

<center>
<figure>
<img src="/assets/post-img/DataStructure/49.png" alt="" width="50%">
</figure>
</center>



## Crossover

selection step > 두개의 솔루션을 mix-up 하는 과정을 crossover 이라고 한다.

해결책에서 가장 좋은 파트만 선택해서 새로운 해결을 생성함하지만 가장 안좋은 파트만 섞어서 하는 경우 가장 안좋은 경우도 발생할 수 있음(단점이 있음)


### One point crossover

일부 포인트를 랜덤하게 기점을 고름으로써 일부 조합을 만들어서 생성함

<center>
<figure>
<img src="/assets/post-img/DataStructure/50.png" alt="" width="50%">
</figure>
</center>


### Multi point crossover

포인트를 여러개로 정하는 것함으로써 항상 2개의 솔루션을 구할 수 있음

<center>
<figure>
<img src="/assets/post-img/DataStructure/51.png" alt="" width="50%">
</figure>
</center>


#### 문제점

- 같은 도시를 두번 갈 수 있는 문제가 발생할 수 있음, 가끔가다가 **invalid soultion** 을 생성하는 경우가 있음

1. 어떻게 대처하는가? 계속해서 그냥 하면됨. 어차피 mutation 을 할것... > 나중에 고르자
2. 조심스럽게 crossover 를 진행하자

맞는 방법이 있는 것이 아니기에 상황에 따라 진행하자!

---
layout: post
title: Data Structure, Mutation
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Mutation

현재 가지고 있는 genotype에 변형을 주어 새로운 답을 만들어내는 것

- hill-climbing: 목적을 달성하기 위해서 최고인 것만 가는것으로 지역 최적점에 빠지는 문제를 해결하기 위해  mutation을 이용 > 다양한 방법을 만들수 없음
- diversity of the population을 없애버린 경우에는 어떻게?
  - 여러가지 selected 과정을 했을 경우 좋은 것들이 선택되도록 하는 경우가 많음
  - cross-over의 답을 mutation 하는 것

### Mutation step

#### random mutations

<center>
<figure>
<img src="/assets/post-img/DataStructure/52.png" alt="" width="50%">
</figure>
</center>

- distribution 을 이용하여 랜덤번호를 선택
- categoriacal variables 인 경우는 같게 하거나 카테고리 전체를 가져와서 사용

#### swap mutations

<center>
<figure>
<img src="/assets/post-img/DataStructure/53.png" alt="" width="50%">
</figure>
</center>

- 가장 보편적으로 사용할 수 있는 방법
- 두개를 랜덤하게 결정해서 바꾸는 것
- 이전 방법보다 invalid 방법이 적게 나옴

#### repair

- invalid한 해답이 나오는 경우 어떻게 고쳐야하는가?
- random  repair until the solution becomes a feasible soultion
- Greedy repair with heuristics

---
layout: post
title: Data Structure, Traveling Salesman Problem
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Traveling Salesman Problem(TSP)

여러군데 도시를 많이 다니면서도 돌아다니는 비용을 줄여야하는 이슈

- genotype : 방문하는 순서 (일정)
- O(N!)

### Terminology of genetic algorithm

<center>
<figure>
<img src="/assets/post-img/DataStructure/46.png" alt="" width="50%">
</figure>
</center>

- GA : 풀어야 하는 문제
- Genc: 광주, 서울 방문하는 도시
- Genotype: 방문 순서
- Encoding: 문제 해결에 대한 표현 방법 ex)서울을 1로 설정
- Phenotype: 해결방법의 가치, 평가 방법 ex)서울-대구 방법, 대구-서울 가치판단
- Population: 여러 방법들의 집합
- Initializtion: 많은 해결방법 중에서 처음으로 사용하는 방법
- selection: 가장 좋아보이는 녀석을 찾으면 됨
- offspring production: selection sub-set 찾아서 또 거기서 해답을 찾음
- environment: 해결책에 대한 phenotype 을 구하는 것 > trip cost calculator

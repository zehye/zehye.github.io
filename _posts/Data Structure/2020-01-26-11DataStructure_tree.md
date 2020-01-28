---
layout: post
title: Data Structure, Tree as an Abstract Data Type and Structure
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Tree as an Abstract Data Type

- 트리는 Abstract Data Type
- 데이터가 저장되는 구조가 tree structure로 저장된다
- `traverse`를 다양하게 정의할 수 있다
  - traverse: 속에 있는 데이터를 모두 끄집어내는 것

<center>
<figure>
<img src="/assets/post-img/DataStructure/18.png" alt="" width="50%">
</figure>
</center>

만약 linked list의 경우 우리가 리스트 안의 데이터를 모두 꺼내온다고 하면 알아서 순서대로 데이터를 꺼내올 것이다. 그러나 트리의 경우 일직선의 구조가 아니기 때문에 데이터를 하나하나 꺼낼때마다 다양한 변종이 생긴다. 즉 traverse를 다양하게 정의할 수 있다.


### why do we use trees?

사용예제> 조직도, 회사의 계좌, 지휘체계 등

명확한 **Divide and Conquer** 이 가능하다! 즉, 각각의 브랜치마다 데이터의 특성을 반영해 Divide하고 저장하는 문제를 풀어나가는 방식


### Structure of stored data

<center>
<figure>
<img src="/assets/post-img/DataStructure/19.png" alt="" width="50%">
</figure>
</center>

- Linked list
  - Next가 1개, 즉 1개의 Node만 가질 수 있다. > 1의 제곱


<center>
<figure>
<img src="/assets/post-img/DataStructure/20.png" alt="" width="50%">
</figure>
</center>

- Tree
  - Next가 4개, 즉 1개의 Node는 하위에 4개의 Node를 가질 수 있다. > 4의 제곱
  - 훨씬 더 많은 저장공간을 가질 수 있게 된다.
  - 다만, Next가 4개이기 때문에 search하는 과정에서 어떤 Next를 선택할지에 대한 고민이 있다.

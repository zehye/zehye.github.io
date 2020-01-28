---
layout: post
title: Data Structure, Terminologies and Characteristics of tree structure
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Terminologies of tree structure

<center>
<figure>
<img src="/assets/post-img/DataStructure/21.png" alt="" width="50%">
</figure>
</center>

- Node: 하나의 Next 레퍼런스가 가리키는 노드 하나
- Edge: 가리키는 레퍼런스
- Root: Tree의 맨 위 노드 > 반드시 저장해야하는 노드
- Root로 부터 바로 연결된 노드가 있을 경우(Root가 아니어도 상/하위 개념으로 보면 됨)
  - 상위에 있는 노드: 부모(Parent), 하위에 있는 노드: 자식(Child)
- siblings: 동일한 부모를 가진 자식의 집합
- Leaves, Terminal Node: 트리 구조에서 더이상 Next를 가지지 않는 경우(garbage값을 가진 경우)
- Internal Node: Terminal Node가 아닌 경우


<center>
<figure>
<img src="/assets/post-img/DataStructure/22.png" alt="" width="50%">
</figure>
</center>

- Descendants Node: 부모 아래에 있는 모든 자식 노드들
- Ancestors Node: 특정 노드의 선조들
- Path: Root위치에서 특정 노드까지의 최단 거리(Edge)

<center>
<figure>
<img src="/assets/post-img/DataStructure/23.png" alt="" width="50%">
</figure>
</center>

- Depth and Level: 특정 노드의 Depth는 Path의 길이
- Height: 가장 긴 Path의 길이
- Degree: 특정 노드가 가진 Next의 갯수
- Size: 노드(저장된 데이터)의 갯수


#### Full Tree

삼각형 모양의 트리

<center>
<figure>
<img src="/assets/post-img/DataStructure/24.png" alt="" width="50%">
</figure>
</center>


#### Complete Tree

바로 직전 Depth까지는 Full Tree structure<br>
왼쪽에서부터 노드를 하나하나 채워나가는 과정이라면 Complete tree structure

<center>
<figure>
<img src="/assets/post-img/DataStructure/25.png" alt="" width="50%">
</figure>
</center>




## Characteristics of tree structure

<center>
<figure>
<img src="/assets/post-img/DataStructure/26.png" alt="" width="50%">
</figure>
</center>

- Number of edges = Number of nodes - 1
- Depth of root = 0
- Height of root = height of tree
- Maximum number of nodes at level i with degree d = d의 i승
- Maximum number of leaves with height h and degree d = d의 h승

<center>
<figure>
<img src="/assets/post-img/DataStructure/27.png" alt="" width="50%">
</figure>
</center>

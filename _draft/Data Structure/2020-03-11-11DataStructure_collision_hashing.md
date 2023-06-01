---
layout: post
title: Data Structure, Collision resolution of hashing and Deletion in open addressing hash table
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Collision resolution of hashing

### Load Factor: Load factor is often the determinant of the hash performance

테이블에 얼마나 붐비게 들어있는지를 의미 = N/S
- N: size of the stored entries
- S: Size of the hash table
- 중요한 이유?
  - qualities of the hash function or uniformity
  - collision

#### Closed addressing

- 하나의 index에 여러 key가 존재할 수 있음
- 여러 key를 저장하기 위해서 Linked List를 이용
- 가장 최악의 케이스
  - 만약 모든 엔트리가 같은 index를 가지고 있는 경우 linked list를 순환하는 것과 같음
- Cosidering the l oad factor
  - Load factor > 1 is possible
  - every  index has one or more entires


#### Open addressing

- 하나의 index는 한개의 key만 존재함
- Linear probing
  - index = (f(key) + i) mod s
- Quadratic probing

<center>
<figure>
<img src="/assets/post-img/DataStructure/68.png" alt="" width="50%">
</figure>
</center>

- index = f(key) + i + i**2) mod s
- 큰 데이터 경우엔 뭉쳐있는 구간이 있는데 이때는 i 씩 이동하는게 큰 의미가 없기 때문에 최대한 충돌 구역에서 벗어나게 하는게 방범


## Deletion in open addressing hash table

### Closed addressing

- linked list를 찾아서 삭제하면됨

### Open addressing

- 해당 index 에 방문하고 만약 그 index 면 삭제 아니면  probing method 를 이용하여 그 index 를 찾아서 삭제하면됨
- 여기서 문제가 발생!

<center>
<figure>
<img src="/assets/post-img/DataStructure/67.png" alt="" width="50%">
</figure>
</center>

중간값이 사라지는 경우 11 index 에 접근할 수 있는 방법이 없음. 따라서 open addressing 은 다른 삭제 방법을 이용!

- Lazy deletion
  - 삭제해야할 index 에 삭제 했다는 필드를 추가해서 표시만하고 실제로 데이터에서는 삭제하지 않음
  - 해당 데이터를 유지하다가 데이터를 옮길때 해당 데이터를 삭제해서 처리함



## Managing the Size of Hash Table

무엇을 해도 한계가 있음! rehashing을 하거나 저장공간을 늘리는 방법을 사용할 수 밖에 없다.

<center>
<figure>
<img src="/assets/post-img/DataStructure/69.png" alt="" width="50%">
</figure>
</center>
<center>
<figure>
<img src="/assets/post-img/DataStructure/70.png" alt="" width="50%">
</figure>
</center>

---
layout: post
title: Data Structure, O(N^2) Sorting
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## O(N^2) Sorting

- Worst case
- without divide and conquer approach
- Sequential comparisons with two index
  - nested loop that ranges
  - outer loop: the first to the end
  - inner loop: from the outer loop's index to end
- Variants
  - Insertion Sort
  - Selection Sort
  - Bubble Sort
- Prods and cons
  - 단점: 시간 복잡도
  - 장점: 구현이 쉽다.

### Selection sort algorithm

```python
def selection_sort(lst):
    for i in range(len(lst)):
        for j in range(i+1,  len(lst)):
            if lst[i] < lst[j]:
                lst[i], lst[j] = lst[j], lst[i]
    return lst

selection_sort(lst)
[1, 9, 3, 4, 2, 5]
[1, 2, 9, 4, 3, 5]
[1, 2, 3, 9, 4, 5]
[1, 2, 3, 4, 9, 5]
[1, 2, 3, 4, 5, 9]
[1, 2, 3, 4, 5, 9]
```

- Total iteration

<center>
<figure>
<img src="/assets/post-img/DataStructure/54.png" alt="" width="50%">
</figure>
</center>


### Bubble sort algorithm

<center>
<figure>
<img src="/assets/post-img/DataStructure/55.png" alt="" width="50%">
</figure>
</center>

```python
def bubble_sort(lst):
    length = len(lst) - 1
    for i in range(length):
        for j in range(length-i):
            if lst[j] > lst[j+1]:
                lst[j], lst[j+1] = lst[j+1], lst[j]
        print(lst)
    return lst

bubble_sort(lst)
[2, 3, 4, 1, 5, 9]
[2, 3, 1, 4, 5, 9]
[2, 1, 3, 4, 5, 9]
[1, 2, 3, 4, 5, 9]
[1, 2, 3, 4, 5, 9]
```

### Insertion sort algorithm

<center>
<figure>
<img src="/assets/post-img/DataStructure/56.png" alt="" width="50%">
</figure>
</center>

```python
def insertion_sort(lst):
    for i in range(len(lst)):
        key = lst[i]
        # i - 1부터 시작해서 왼쪽으로 하나씩 확인
        # 왼쪽 끝까지(0번 인덱스) 다 봤거나
        # key가 들어갈 자리를 찾으면 끝냄
        j = i - 1
        while j >= 0 and lst[j] > key:
            lst[j + 1] = lst[j]
            j = j - 1
        # key가 들어갈 자리에 삽입
        # 왼쪽 끝까지 가서 j가 -1이면 0번 인덱스에 key를 삽입
        lst[j + 1] = key
        print(lst)
    return lst

[9, 2, 3, 4, 1, 5]
[2, 9, 3, 4, 1, 5]
[2, 3, 9, 4, 1, 5]
[2, 3, 4, 9, 1, 5]
[1, 2, 3, 4, 9, 5]
[1, 2, 3, 4, 5, 9]
```

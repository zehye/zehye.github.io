---
layout: post
title: Data Structure, O(NlogN) Sorting
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## O(NlogN) Sorting

- Worst case O(N^2)
- Averabe case O(NlogN)
- with divided and conquer approch
- Divide the target sequence into multiple sequences
  - Recursively perform sorting of the sub-sequences
  - Problem is
    - How to divide
- Variants
  - **Quick Sort**
  - **Heap Sort**
  - **Merge Sort**
- Pros and Cons?
  - 단점: bad division leads into O(N) time complexity
  - 장점: relatively good time complexity

### Merge Sort

<center>
<figure>
<img src="/assets/post-img/DataStructure/57.png" alt="" width="50%">
</figure>
</center>

```python
def merge_sort(lst):
    if len(lst) == 1:
        return lst

    sub_lst_sort1 = []
    sub_lst_sort2 = []

    # decomposition     
    for i in range(len(lst)):
        if len(lst) / 2 > i:
            sub_lst_sort1.append(lst[i])
        else:
            sub_lst_sort2.append(lst[i])

    # recurstion     
    sub_lst_sort1 = merge_sort(sub_lst_sort1)
    sub_lst_sort2 = merge_sort(sub_lst_sort2)


    # aggreagtion     
    idx_count1, idx_count2 = 0, 0
    for i in range(len(lst)):
        print(f'{sub_lst_sort1} | {sub_lst_sort2}')
        if idx_count1 == len(sub_lst_sort1):
            lst[i]  = sub_lst_sort2[idx_count2]
            idx_count2 += 1

        elif idx_count2 == len(sub_lst_sort2):
            lst[i] = sub_lst_sort1[idx_count1]
            idx_count1 += 1

        elif sub_lst_sort1[idx_count1] > sub_lst_sort2[idx_count2]:
            lst[i] = sub_lst_sort2[idx_count2]
            idx_count2 += 1

        else:
            lst[i] = sub_lst_sort1[idx_count1]
            idx_count1 += 1
        print(lst)

    return lst
```


### Heap Sort

<center>
<figure>
<img src="/assets/post-img/DataStructure/58.png" alt="" width="50%">
</figure>
</center>

- Basic idea
  - QuickSort(Sequence)
  - Given a sequence
  - Select a pivot
  - Pivot = a threshold to divide the sequence into two sub-sequences
- Divide the sequence into two sub-sequences
  - Sequence with values less than the pivot
  - Sequence with values greater than the pivot
- Return
  - QuickSort(sequence with less) + Pivot + QuickSort(sequence with greater)
  - Merge sort forces to divide the sequence in the middle
  - Always the similar size of the sub-sequence
- This divides the sequence with the pivot

### Quick Sort

<center>
<figure>
<img src="/assets/post-img/DataStructure/59.png" alt="" width="50%">
</figure>
</center>

- Basic idea
  - Given a sequence
  - Select a pivot
    - Pivot = a threshold to divide the sequence into two sub-sequences
- Divide the sequence into two sub-sequences
  - Sequence with values less than the pivot
  - Sequence with values greater than the pivot
- Return
  - QuickSort(sequence with less) + Pivot + QuickSort(sequence with greater)
- Merge sort forces to divide the sequence in the middle
  - Always the similar size of the sub-sequence
- This divides the sequence with the pivot selection
- Pivot 을 잘못 선택하는 경우 최악의 시나리오 O(N^2)

```python
def quick_sort(seq, pivot = 0):
    if len(seq) <= 1:
        return seq

    pivot_value = seq[pivot]
    less_lst = []
    grater_lst = []

    for i in range(len(seq)):
        if i == pivot:
            continue
        elif seq[i] > pivot_value:
            grater_lst.append(seq[i])
        elif seq[i] <= pivot_value:
            less_lst.append(seq[i])

    result = quick_sort(less_lst) + [pivot_value] + quick_sort(grater_lst)
    return result
```

---
layout: post
title: Data Structure, O(N) Sorting
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## O(N) Sorting

- Not comparison-based approach
  - The best performance of the comparison based approach is O(NlogN)
  - Therefore, should not be based upon comparisons
- Rather based upon counting and numeric properties
- Variants
  - **Radix Sort**
  - **Count Sort**
- Pros and Cons?
  - 단점: 가정이 맞아야하며 comparison-based가 아니어야함
  - 장점: 가장 좋은 시간 복잡도

## Counting  Sorting

<center>
<figure>
<img src="/assets/post-img/DataStructure/60.png" alt="" width="50%">
</figure>
</center>

- Assumption
  - The sequence contains integers(자연수) ranging from 0 to K
  - Count the occurrence and produce a sequence based upon the counts

- 시간 복잡도
  - O(N+R)
  - R = the range of the sequence values
  - N = the size of the sequence

```python
def counting_sort(seq):
    # prepareing the couting space
    max = -9999
    min = 9999
    for i in range(len(seq)):
        if seq[i] > max:
            max = seq[i]
        if seq[i] < min:
            min = seq[i]
    counting = list(range(max-min+1))
    for i in range(len(counting)):
        counting[i] = 0

    # perform couting     
    for i in range(len(seq)):
        value = seq[i]
        counting[value-min] = counting[value-min] + 1

    cnt = 0
    for i in range(max-min+1):
        for j in range(counting[i]):
            seq[cnt] = i + min
            cnt += 1
    return seq
```


### Radix Sort

<center>
<figure>
<img src="/assets/post-img/DataStructure/61.png" alt="" width="50%">
</figure>
</center>

- Assumption
  - The sequence contains integers
  - Sort from the least important digit to the most important digit

- Sort from the least important digit to the most important digit
  - Sort from 1000 **2** to **1** 0002

- 시간복잡도
  - O(ND)
  - D = the digit number of the largest value
  - N = the size of the sequence
  - Is this a good approach?

```python
import math

def radix_sort(seq):
    # finding the digit number
    max = - 99999
    for i in range(len(seq)):
        if seq[i] > max:
            max = seq[i]
    d = int(math.log10(max))
    print(d)


    for i in range(0, d+1):
        # palcing values into bucket      
        buckets = []
        for j in range(0, 10):
            buckets.append([])
        for j in range(len(seq)):
            digit = int(seq[j]  /  math.pow(10, i)) % 10
            buckets[digit].append(seq[j])
        print(buckets)
        # printing the partially sorted values              
        cnt = 0
        for j in range(0, 10):
            for k in range(len(buckets[j])):
                seq[cnt] = buckets[j][k]
                cnt = cnt +1
    return seq
```


#### Performance of sorting algorithms

<center>
<figure>
<img src="/assets/post-img/DataStructure/62.png" alt="" width="50%">
</figure>
</center>

- In the real world
  - Many people do not concern the time complexity of the sorting
- Why?
  - Most of time, people rely on the database and “DESC” and “ASC”
  - Most of time, people do not give too much thought on this issue
- Not a good idea
  - You need to consider the cost of your system
  - Development
  - Mainten

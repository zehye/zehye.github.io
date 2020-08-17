---
layout: post
title: Swift, LeetCode - Contains Duplicate
category: Algorithm
tags: [Algorithm]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## Contains Duplicate

```
Given an array of integers, find if the array contains any duplicates.
Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.
```

정수 배열이 주어지면 배열에 중복 항목이 있는지 확인한다.<br>
배열안의 값이 두 번 이상 나타나면 함수는 true를 반환해야하며 모든 요소가 고유하면 false를 반환해야 한다.


### Example

```
Input: [1,2,3,1]
Output: true

Input: [1,1,1,3,3,4,3,2,4,2]
Output: true
```
배열 안의 값이 2번 이상 나오기 때문에 true값을 반환한다.

```
Input: [1,2,3,4]
Output: false
```
배열안의 모든 값이 고유하면 false값을 반환한다.


```swift
class Solution {
    func containsDuplicate(_ nums: [Int]) -> Bool {
        var set = Set<Int>()

        for num in nums {
            if set.contains(num) {
                return true
            }
            set.insert(num)
        }
        return false
    }
}
```


더 간단한 코드

```swift
class Solution {
    func containsDuplicate(_ nums: [Int]) -> Bool {
        if nums.count < 2 {
            return false
        }
        return Set(nums).count != nums.count
    }
}
```

---
layout: post
title: Swift, LeetCode - Two Sum
category: Algorithm
tags: [Algorithm]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## Two Sum

```
Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
```

정수 배열이 주어지며 두 숫자를 더해 타겟이 되는 인덱스의 숫자를 반환하라.<br>
각 입력에는 하나의 답만이 존재하며 동일한 원소가 두번 나오지 않는 다는 것을 가정하라.

### Example

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

1. 배열 [2, 7, 11, 15]와 목표값 9가 주어진다.
2. 정수 배열 중 두개의 숫자를 골라 목표값을 만족하는 인덱스를 리턴한다.
3. 따라서 위 배열에서는 2와 7이 목표값 9를 만족하는 숫자가 된다.
4. 2와 7의 인덱스 [0,1]을 리턴해주면 된다.

```swift
class Solution {
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        for i in 0..<nums.count {
            for j in i..<nums.count {
                if i != j, nums[i] + nums[j] == target {
                    return [i,j]
                }
            }
        }
        return [0]
    }
}
```

- 이중 for문을 돌면서 하나씩 다 맞춰보는 방식으로 문제를 풀었다.
- 시간복잡도: O(n^2) & 공간복잡도: O(1)

시간복잡도를 해결해야하는데, 좋은 방법으로는 해시맵을 사용하는 방법이라고 한다.(조금더 공부가 필요한 것 같다)

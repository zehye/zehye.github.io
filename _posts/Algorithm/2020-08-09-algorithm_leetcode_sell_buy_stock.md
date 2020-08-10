---
layout: post
title: Swift, LeetCode - Best Time to Buy and Sell Stock
category: Algorithm
tags: [Algorithm]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      

<hr>

## Best Time to Buy and Sell Stock

```
Say you have an array for which the ith element is the price of a given stock on day i.
If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.
Note that you cannot sell a stock before you buy one.
```

i번째 요소 = i번째 일(day)인 배열을 가지고 있다<br>
최대 한 번의 거래만 완료할 수 있다(즉, 주식 하나를 사고 1 주를 판매) <br>
이때 최대 수익을 찾는 알고리즘을 설계하십시오(제일 싼값에 사고 제일 비싼값에 팔아야 한다)<br>
즉, 주식을 구입하기 전에는 주식을 팔 수 없습니다. (사고 > 팔고)


### Example

```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```

1. 7이 될 수 없는 이유는 우선 제일 싼 값에 사야하는데 7이 제일 크기 때문이다.
2. 따라서 제일 작은 값이 오는 2번째 날의 값(1) 그 이후 제일 큰 값인 5번째 날의 값(6)이 최대 수익이다.
3. 따라서 max profit = (6-1) = 5

```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

1. 작은 작은 수인 1이 맨 마지막에 있기때문에 그 어느것도 최대 수익을 갖지 못한다.
2. 따라서 max profit = 0


```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        var min = prices[0]  // 첫번째 초기값을 셋팅(제일 작은수가 됨)
        var num = 0  // 우리가 구하고자하는 숫자를 0으로 셋팅 (max profit값이 됨)

        for i in prices {
            if i < min {  // prices 배열을 돌면서 초기값과 비교
                min = i   // 제일 작은수를 찾는다
            } else if (i-min) > num {
                num = i - min
            }
        }
        return num
    }
}
```

- Runtime 에러가 뜨는 것을 발견했다....

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {

        if prices.isEmpty {
            return 0
        }

        var min = prices[0]
        var num = 0

        for i in prices {
            if i < min {
                min = i
            } else if (i-min) > num {
                num = i - min
            }
        }
        return num
    }
}
```

- 위 코드를 추가해주면 성공 

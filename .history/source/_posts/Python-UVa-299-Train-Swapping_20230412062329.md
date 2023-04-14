---
title: Python UVa 299 - Train Swapping
date: 2023-03-19 21:44:12
tags:
  - UVa
  - UVa 一星
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 299 - Train Swapping 解題心得
description: Python UVa 299 - Train Swapping 解題心得
---

# Python UVa 299 - Train Swapping

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=0&problem=235&mosmsg=Submission+received+with+ID+28320012) - UVa 299 - Train Swapping



## 題意
題目寫了一大堆，簡單來說就是使用[Bubble Sort](https://www.geeksforgeeks.org/bubble-sort/)把火車由小到大做排序

#### Sample Input 
`3`
`3`
`1 3 2`
`4`
`4 3 2 1`
`2`
`2 1`

#### Sample Output 
`Optimal train swapping takes 1 swaps.`
`Optimal train swapping takes 6 swaps.`
`Optimal train swapping takes 1 swaps.`

---
## 解題思路
使用[Bubble Sort](https://www.geeksforgeeks.org/bubble-sort/)來由小到大排序陣列，紀錄Bubble Sort中總共交換了幾次即可:D


## Python程式碼
```python
# UVa 299 - Train Swapping
def solve(n, nums):
    count = 0
    for i in range(n-1):
        for j in range(n-i-1):
            if nums[j] > nums[j+1]:
                nums[j], nums[j+1] = nums[j+1], nums[j]
                count += 1
    return f'Optimal train swapping takes {count} swaps.'

T = int(input())
for t in range(T):
    n = int(input())
    nums = list(map(int, input().split()))

    print(solve(n, nums))
# Accepted	PYTH3	0.010
```
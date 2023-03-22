---
title: Python UVa - 11240 - Antimonotonicity
date: 2023-03-22 10:21:10
tags:
  - UVa
categories:
  - [解題報告, UVa]
  - [CPE歷屆, 2023/03/21]
excerpt: Python UVa - 11240 - Antimonotonicity 解題報告
description: Python UVa - 11240 - Antimonotonicity 解題報告
keywords: Python UVa 11240, UVa 11240, UVa Python, Antimonotonicity, Antimonotonicity Python, UVa - 11240 - Antimonotonicity, Python UVa - 11240 - Antimonotonicity
---
# Python UVa - 11240 - Antimonotonicity

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&category=0&problem=2181&mosmsg=Submission+received+with+ID+28325133) - UVa - 11240 - Antimonotonicity 

這題是2023/3/21 CPE 的第四題，我覺得這題很像買股票的題目，一樣是找高點和低點，直接用迴圈跑一次就好了

## 題意
輸入一個陣列，代表`Fred`，算出在`Fred`中，符合`Mary`最長的子序列(subsequence)，[subsequence定義](https://web.ntnu.edu.tw/~algo/Subsequence.html)

Mary需要符合以下條件
`Mary[0] > Mary[1] < Mary[2] > Mary[3] < ...`

{% note primary %}
- `子序列(subsequence)`定義最好看一下
{% endnote %}

#### Sample Input 
`4`
`5 1 2 3 4 5`
`5 5 4 3 2 1`
`5 5 1 4 2 3`
`5 2 4 1 3 5`

#### Sample Output 
`1`
`2`
`5`
`3`

---
## 解題思路



## 程式碼
```python
# UVa - 11240 - Antimonotonicity
# 2023/03/21/ CPE - 4
def solve(n, nums):
    current = 1 
    ans = 1
    for i in range(1, n):
        if current == 1:
            if nums[i-1] - nums[i] > 0: 
                ans += 1
                current *= -1
        else:
            if nums[i-1] - nums[i] < 0: 
                ans += 1
                current *= -1
    return ans

T = int(input())
for t in range(T):
    nums = list(map(int, input().split()))

    print(solve(nums[0], nums[1:]))
# Accepted	PYTH3	0.460
```